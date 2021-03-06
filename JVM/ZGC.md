# ZGC

作者：RednaxelaFX
链接：https://www.zhihu.com/question/287945354/answer/458761494
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

ZGC 所采用的算法就是 Azul Systems 很多年前提出的 Pauseless GC

虽然 Oracle 出的各种介绍资料上都完全没有提及 ZGC 与 Azul 的 Pauseless GC（下面简称 Azul PGC）之间的关系，而且我们从外部也无法证实或否认 Oracle GC 团队在研发 ZGC 的时候是否参考了 Azul 的论文，所以还不至于扣上抄袭啊克隆啊之类的帽子，但就结果来看 ZGC 确实就是换了一通术语、纯软件实现的 Azul PGC

### 原理

Azul PGC 简单来说是：它是一个 mark-compact GC，但是 GC 过程中所有的阶段都设计为可以并发的，包括移动对象的阶段，所以 GC 正常工作的时候除了会在自己的线程上吃点 CPU 之外并不会显著干扰应用的运行。

为了实现上方便，PGC 虽然算法上可以做成完全并发，Azul PGC 在 Azul VM 里的实现还是有三个非常短暂的 safepoint，其中第一个是做根集合（root set）扫描，包括全局变量啊线程栈啊啥的里面的对象指针，但不包括 GC 堆里的对象指针，所以这个暂停就不会随着 GC 堆的大小而变化（不过会根据线程的多少啊、线程栈的大小之类的而变化）。另外两个暂停也同样不会随着堆大小而变化。

这种并发算法的核心思想就是：

（1）在标记阶段，与其说是标记对象（记录对象是否已经被标记），不如说是标记指针（记录 GC 堆里的每个指针是否已经被标记）。

这就与传统的三色标记对象的 GC 算法有非常大的区别，虽然两者从收敛性上看是等价的——最终所有对象以及所有指针都会被遍历过。

（2）在标记和移动对象的阶段，每次从 GC 堆里的对象的引用类型字段里读取一个指针的时候，这个指针都会经过一个“Loaded Value Barrier”（LVB）。这是一种“Read Barrier”（读屏障），会在不同阶段做不同的事情。

最简单的事情就是，在标记阶段它会把指针标记上并把堆里的这个指针给“修正”到新的标记后的值；而在移动对象的阶段，这个屏障会把读出的指针更新到对象的新地址上，并且把堆里的这个指针“修正”到原本的字段里。这样就算 GC 把对象移动了，读屏障也会发现并修正指针，于是应用代码就永远都会持有更新后的有效指针，而不需要通过 stop-the-world 这种最粗粒度的同步方式来让 GC 与应用之间同步。

（3）LVB 中有一点很重要，就是“self healing”性质：如果堆上有指针当前处于“尚未更新”的状态，一旦经过 LVB 之后就会被就地更新，于是在同一个 GC 周期内再次访问这个字段的话就不需要再修正了。

这样 LVB 带来的性能开销（吞吐量的下降）就是非常短暂的，而不像 Shenandoah GC 所使用的 Brooks indirection pointer 那样一直都慢。

### 实现

https://mp.weixin.qq.com/s/Pzd0OoxbW1tg59pZObu0pg

ZGC 依然没有做到整个 GC 过程完全并发执行，依然有 3 个 STW 阶段，其他 3 个阶段都是并发执行阶段：

（1）Pause Mark Start（STW）

这一步就是初始化标记，和 CMS 以及 G1 一样，主要做 Root 集合扫描，「GC Root 是一组必须活跃的引用，而不是对象」。

例如：活跃的栈帧里指向 GC 堆中的对象引用、Bootstrap/System 类加载器加载的类、JNI Handles、引用类型的静态变量、String 常量池里面的引用、线程栈/本地(native)栈里面的对象指针等，但不包括 GC 堆里的对象指针。所以这一步骤的 STW 时间非常短暂，并且和堆大小没有任何关系。不过会根据线程的多少、线程栈的大小之类的而变化。

（2）Concurrent Mark/Remap（STW）

第二步就是并发标记阶段，这个阶段在第一步的基础上，继续往下标记存活的对象。并发标记后，还会有一个短暂的暂停（Pause Mark End），确保所有对象都被标记（因为并发时，这是一个追赶游戏）。

（3）Concurrent Prepare for Relocate（STW）

即为 Relocation 阶段做准备，选取接下来需要标记整理的 Region 集合，这个阶段也是并发执行的。接下来又会有一个 Pause Relocate Start 步骤，它的作用是只移动 Root 集合对象引用，所以这个 STW 阶段也不会停顿太长时间。

（4）Concurrent Relocate

最后，就是并发回收阶段了，这个阶段会把上一阶段选中的需要整理的 Region 集合中存活的对象移到一个新的 Region 中（这个行为就叫做「Relocate」，即重新安置对象），如上图所示。Relocate 动作完成后，原来占用的 Region 就能马上回收并被用于接下来的对象分配。细心的同学可能有疑问了，这就完了？Relocate 后对象地址都发生变化了，应用程序还怎么正常操作这些对象呢？这就靠接下来会详细说明的 Load Barrier 了

### Colored Pointers

Colored Pointers，即颜色指针是什么呢？如下图所示，ZGC 的核心设计之一。以前的垃圾回收器的 GC 信息都保存在对象头中，而 ZGC 的 GC 信息保存在指针中。每个对象有一个 64 位指针，这 64 位被分为：

18 位：预留给以后使用；
1 位：Finalizable 标识，次位与并发引用处理有关，它表示这个对象只能通过 finalizer 才能访问；
1 位：Remapped 标识，设置此位的值后，对象未指向 relocation set 中（relocation set 表示需要 GC 的 Region 集合）；
1 位：Marked1 标识；
1 位：Marked0 标识，和上面的 Marked1 都是标记对象用于辅助 GC；
42 位：对象的地址（所以它可以支持 2^42=4T 内存）：
通过对配置 ZGC 后对象指针分析我们可知，对象指针必须是 64 位，那么 ZGC 就无法支持 32 位操作系统，同样的也就无法支持压缩指针了（CompressedOops，压缩指针也是 32 位）。

### Load Barriers

这个应该翻译成读屏障（与之对应的有写屏障即 Write Barrier，之前的 GC 都是采用 Write Barrier，这次 ZGC 采用了完全不同的方案），这个是 ZGC 一个非常重要的特性。

在标记和移动对象的阶段，每次「从堆里对象的引用类型中读取一个指针」的时候，都需要加上一个 Load Barriers。

那么我们该如何理解它呢？看下面的代码，第一行代码我们尝试读取堆中的一个对象引用 obj.fieldA 并赋给引用 o（fieldA 也是一个对象时才会加上读屏障）。如果这时候对象在 GC 时被移动了，接下来 JVM 就会加上一个读屏障，这个屏障会把读出的指针更新到对象的新地址上，并且把堆里的这个指针“修正”到原本的字段里。这样就算 GC 把对象移动了，读屏障也会发现并修正指针，于是应用代码就永远都会持有更新后的有效指针，而且不需要 STW。

那么，JVM 是如何判断对象被移动过呢？就是利用上面提到的颜色指针，如果指针是 Bad Color，那么程序还不能往下执行，需要「slow path」，修正指针；如果指针是 Good Color，那么正常往下执行即可

「扩展阅读」：既然低 42 位指针可以支持 4T 内存，那么能否通过预约更多位给对象地址来达到支持更大内存的目的呢？答案肯定是不可以。因为目前主板地址总线最宽只有 48bit，4 位是颜色位，就只剩 44 位了，所以受限于目前的硬件，ZGC 最大只能支持 16T 的内存，JDK13 就把最大支持堆内存从 4T 扩大到了 16T。

### 扩展阅读

新一代垃圾回收器 ZGC 的探索与实践
https://mp.weixin.qq.com/s/ag5u2EPObx7bZr7hkcrOTg
