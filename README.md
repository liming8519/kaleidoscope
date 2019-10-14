# Introduction
[Paxos made simple](http://lamport.azurewebsites.net/pubs/paxos-simple.pdf)

[Leslie Lamport](http://www.lamport.org/)

[2013 ACM Turing Award](https://amturing.acm.org/award_winners/lamport_1205376.cfm)

[关于Paxos的历史](关于Paxos的历史.md)

# Summary
* [0 `Introduction`](README.md)
* [1 `Algorithm`](README.md)
  * [1.1 `CAP`理论](algorithm/CAP.md)
  * [1.2 `ZAB`协议](algorithm/ZAB协议.md)
  * [1.3 分布式锁](algorithm/分布式锁.md)
  * [1.4 `RAFT`算法](algorithm/raft-zh_cn.md)
  * [1.5 3分钟理解`PAXOS`算法](algorithm/PAXOS.md)
  * [1.6 `The Art of Multiprocessor Programming, Revised Reprint`](algorithm/TAMP.md)
  * [1.7 `BFPRT`算法](algorithm/BFPRT.md)
  * [1.8 `FisherYates`洗牌算法](algorithm/FisherYates洗牌算法.md)
  * [1.9 `Dapper`笔记](algorithm/Dapper笔记.md)
  * [1.10 算法概论](algorithm/算法概论.md)
* [2 Java](README.md)
  * [2.1 `JavaGC`优化](java/从实际案例聊聊Java应用的GC优化.md)
  * [2.2 设计模式](java/设计模式23.md)
  * [2.3 `EffectiveJava`](java/EffectiveJava.md)
  * [2.4 `Java`学习笔记](java/Java学习笔记.md)
  * [2.5 `JDK`相关](java/JDK相关.md)
  * [2.6 阿里`Java`编码规范整理](java/AlibabaJavaCodingGuidelines.md)
  * [2.7 深入了解`Java`程序执行顺序](java/Java程序执行次序.md)
  * [2.8 `JDK`定时任务对比](java/JDK定时任务对比.md)
  * [2.9 `Tomcat`中应用加载两次](java/Tomcat中应用加载两次.md)
  * [2.10 动态代理](java/动态代理.md)
  * [2.11 `Cron`大乱斗](java/Cron大乱斗.md)
  * [2.12 `Java`虚拟机](java/Java虚拟机.md)
  * [2.13 负载均衡与高可用](java/负载均衡与高可用.md)
  * [2.14 网络协议](java/网络协议.md)
  * [2.15 Safe Publication and Safe Initialization in Java](java/Safe-Publication-and-Safe-Initialization-in-Java.md)
  * [2.16 `ThreadLocal`解析](java/ThreadLocal解析.md)
  * [2.17 `JUC`解析](java/JUC解析.md)
  * [2.18 码农翻身整理](java/码农翻身整理.md)
  * [2.19 序列化与JSON](java/序列化与JSON.md)
  * [2.20 Java中的位运算](java/Java中的位运算.md)
* [3 `SpringBoot`](README.md)
  * [3.1 `SpringBoot`的`pom.xml`](springboot/SpringBoot-pom-xml.md)
  * [3.2 `SpringBoot`的注解](springboot/SpringBoot注解.md)
  * [3.3 重新定义`SpringCloud`实战笔记](springboot/重新定义SpringCloud实战.md)
  * [3.4 `LogBack`](springboot/LogBack相关.md)
  * [3.5 `Maven`](springboot/Maven相关.md)
  * [3.6 `OAuth2.0`](springboot/OAuth2.0.md)
  * [3.7 `SpringBoot`排错](springboot/SpringBoot排错.md)
  * [3.8 `SpringBoot`排雷现场](springboot/SpringBoot排雷现场.md)
  * [3.9 `SpringBoot`释疑](springboot/SpringBoot释疑.md)
  * [3.10 `SpringBoot`引入`log4j`的配置](springboot/SpringBoot引入log4j的配置.md)
  * [3.11 `SpringBoot`启动脚本](springboot/SpringBoot启动脚本.md)
  * [3.12 `SpringBoot`定时任务](springboot/SpringBoot定时任务.md)
  * [3.13 `SpringBoot`初始化任务](springboot/SpringBoot初始化任务.md)
  * [3.14 `SpringBootRedis`](springboot/SpringBootRedis.md)
  * [3.15 `SpringBootRabbitMQ`](springboot/SpringBootRabbitMQ.md)
  * [3.16 `pom`标签](springboot/packaging为pom标签的用法.md)
  * [3.17 客户端配置更新](springboot/客户端配置更新.md)  
  * [3.18 `SpringBoot`配置优先级](springboot/SpringBoot配置优先级.md)  
  * [3.19 `SpringBoot`自定义注解](springboot/自定义注解.md)  
  * [3.20 `Hystrix+Turbine`](springboot/Hystrix+Turbine.md)  
  * [3.21 过滤器与拦截器](springboot/过滤器与拦截器.md)
* [4 `RabbitMQ`](README.md)
  * [4.1 `RabbitMQ`相关](rabbitmq/RabbitMQ相关.md)
  * [4.2 `RabbitMQ`四种发送方式区别](rabbitmq/RabbitMQ四种发送方式区别.md)
  * [4.3 `RabbitMQ`安装全记录](rabbitmq/RabbitMQ安装全记录.md)
  * [4.4 `RabbitMQ`多地址连接](rabbitmq/RabbitMQ多地址连接.md)
  * [4.5 `RabbitMQConsumer`源码解析](rabbitmq/RabbitMQConsumer源码解析.md)
  * [4.6 `AMQP`协议](rabbitmq/AMQP协议.md)
  * [4.7 消息中间件选型图解](rabbitmq/消息中间件选型图解.md)
  * [4.8 `RabbitMQ`消息存储](rabbitmq/RabbitMQ消息存储.md)
  * [4.9 RabbitMQ延时队列](rabbitmq/RabbitMQ延时队列.md)
* [5 `Shell`](README.md)
  * [5.1 免密登陆](shell/免密登陆.md)
  * [5.2 `iptables`命令](shell/iptables命令.md)
  * [5.3 `scp`命令](shell/scp命令.md)
  * [5.4 `shell`特殊变量的含义](shell/shell特殊变量的含义.md)
  * [5.5 `TCP`最大连接数](shell/TCP最大连接数.md)
  * [5.6 `Linux`命令整理](shell/Linux命令整理.md)
  * [5.7  常用Linux命令](shell/常用Linux命令.md)
  * [5.8  SpringBoot启动脚本](shell/启动脚本.md)
* [6 `ZooKeeper`](README.md)
  * [6.1 `ZooKeeper`集群安装](zookeeper/ZooKeeper集群安装.md)
  * [6.2 `ZooKeeper`权限控制](zookeeper/ZooKeeper权限控制.md)
  * [6.3 `ZooKeeperWatcher`](zookeeper/ZooKeeperWatcher.md)
  * [6.4 `ZooKeeper`连接](zookeeper/ZooKeeper连接.md)
  * [6.5 `ZooKeeper`临时节点之坑](zookeeper/ZooKeeper临时节点之坑.md)
* [7 `Pic`](README.md)
  * [7.1 `Java`相关](pic/Java相关.md)
  * [7.2 `JSON`语法](pic/JSON语法.md)
  * [7.3 其他](pic/其他.md)
* [8 `Scheduler`](README.md)
  * [8.1 定时任务调度框架整理](scheduler/定时任务调度框架整理.md)
  * [8.2 `TBSchedule`](scheduler/TBSchedule.md)
  * [8.3 `Onlinetask-Scheduler`](scheduler/Onlinetask-Scheduler.md)
  * [8.4 调度器负载均衡算法](scheduler/调度器负载均衡算法.md)
  * [8.5 有向无环图检测算法](scheduler/有向无环图检测算法.md)
* [9 `Redis`](README.md)
  * [9.1 缓存更新策略](redis/缓存更新策略.md)
  * [9.2 `Redis`集群搭建](redis/Redis集群搭建.md)
  * [9.3 Redis深度历险](redis/Redis深度历险.md)
* [10 `JSR`](README.md)
  * [10.1 `Java-Servlet-4.0`阅读笔记](JSR/JSR-369-Java-Servlet-4.0阅读笔记.md)
  * [10.2 `JSR133`-内存模型](JSR/JSR133-内存模型.md)
* [11 `MongoDB`](README.md)
  * [11.1`MongoDB`集群与分片](mongodb/MongoDB集群与分片.md)
* [12 `Nacos`](README.md)
  * [12.1 `Nacos`简介](nacos/Nacos简介.md)
* [13 `Eureka`](README.md)
  * [13.1 `Eureka`简介](eureka/Eureka简介.md)
  * [13.2 `Eureka`解析](eureka/Eureka解析.md)
* [14 Code](README.md)
  * [14.1 线程安全的`SDF`](code/线程安全的SDF.md)
  * [14.2 获取本机`IP`](code/获取本机IP.md)
  * [14.3 网络探测：`ping`与`telnet`](code/网络探测：ping与telnet.md)
  * [14.4 令牌桶](code/令牌桶.md)
* [15 Database](README.md)
  * [15.1 An Outline for a Book on InnoDB](database/An Outline for a Book on InnoDB.md)
  * [15.2 MySQL建表关键字说明](database/MySQL建表关键字说明.md)
  * [15.3 MySQL问题整理](database/MySQL问题整理.md)
  * [15.4 字典表设计](database/字典表设计.md)
  * [15.5 树形结构](database/树形结构.md)
  * [15.6 理解MySQL](database/理解MySQL.md)
  * [15.7 密码加盐](database/密码加盐.md)
  * [15.8 对NOT NULL的理解](database/对NOT NULL的理解.md)
* [16 OS](README.md)
  * [16.1 windows相关](OS/windows相关.md)
  * [16.2 Monitor](OS/Monitor.md)
* [17 Network](README.md)
  * [17.1 jsch的使用](network/jsch的使用.md)
  * [17.2 会话管理](network/会话管理.md)
  * [17.3 长短连接](network/长短连接.md)
  * [17.4 SSL工作原理](network/SSL工作原理.md)
  * [17.5 CORS+CSRF+XSS](network/CORS+CSRF+XSS.md)
* [`Other`](README.md)
  * [1 `ActiveMQ`简介](other/ActiveMQ简介.md)
  * [2 `Atom`安装](other/Atom安装.md)
  * [3 `GitBook`](other/GitBook.md)
  * [4 `MarkDown`](other/MD语法.md)
  * [5 `Nginx`](other/Nginx安装.md)
  * [6 OTP动态令牌](other/OTP动态令牌.md)
  * [7 `SublimeText3`快捷键精华版](other/SublimeText3快捷键精华版.md)
  * [8 `Webdis`安装](other/webdis安装.md)
  * [9 `Zabbix`](other/zabbix.md)
  * [10 安装`ES-head`](other/安装ES-head.md)
  * [11 快速熟悉新项目](other/快速熟悉新项目.md)
  * [12 思维导图](other/思维导图.md)
  * [13 思考的乐趣](other/思考的乐趣.md)
  * [14 资源汇总](other/资源汇总.md)
  * [15 git常用操作](other/git常用操作.md)
