### Spring 的事务传播

事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如方法可能继续在现有事务中运行，也可能开启一个新的事务，并在自己的事务运行。

spring 中的事务传播行为可以由传播属性指定。spring 指定了 7 种类传播行为。

（1）REQUIRED（兼容或新建）：

Support a current transaction, create a new one if none exists.

如果有事务在运行，当前的方法就在这个事务内运行，否则就开启一个新的事务，并在自己的事务内运行，默认传播行为

（2）REQUIRED_NEW（新建-挂起）：

Create a new transaction, suspend the current transaction if one exists.

当前方法必须启动新事务，并在自己的事务内运行，如果有事务正在运行，把当前事务挂起

（3）SUPPORTS（或）：

Support a current transaction, execute non-transactionally if none exists.

如果有事务在运行，当前的方法就在这个事务内运行，否则可以不运行在事务中

（4）NOT_SUPPORTED（非-挂起）：

Execute non-transactionally, suspend the current transaction if one exists.

表示该方法不应该运行在事务中。如果存在当前事务，在该方法运行期间，当前事务将被挂起。如果使用 JTATransactionManager 的话，则需要访问 TransactionManager

（5）MANDATORY（与-异常）：

Support a current transaction, throw an exception if none exists.

当前的方法必须运行在事务内部，如果没有正在运行的事务，就会抛出异常

（6）NEVER（非-异常）：

Execute non-transactionally, throw an exception if a transaction exists.

当前方法不应该运行在事务中，如果有运行的事务，就抛出异常

（7）NESTED（嵌套或新建）：

Execute within a nested transaction if a current transaction exists, behave like PROPAGATION_REQUIRED else.

如果有事务在运行，当前的方法就应该在这个事务的嵌套事务内运行。嵌套的事务可以独立于当前事务进行单独地提交或回滚。如果当前事务不存在，那么其行为与 PROPAGATION_REQUIRED 一样。

### 个人理解

| 类型          | 是否需要事务 | 是否共享事务       | 是否新建事务 | 备注       |
| ------------- | ------------ | ------------------ | ------------ | ---------- |
| REQUIRED      | 是           | 是（有就共享）     | 是           | 兼容或新建 |
| REQUIRED_NEW  | 是           | 否（挂起其他）     | 是           | 新建-挂起  |
| SUPPORTS      | 否           | 是（有就共享）     | 否           | 或         |
| NOT_SUPPORTED | 否           | 否（挂起其他）     | 否           | 非-挂起    |
| MANDATORY     | 是           | 是（没有就抛异常） | 否           | 与-异常    |
| NEVER         | 否           | 否（有就抛异常）   | 否           | 非-异常    |
| NESTED        | 是           | 否（嵌套）         | 是           | 嵌套或新建 |

### 扩展阅读

#### 1. Spring 事务传播行为详解

https://segmentfault.com/a/1190000013341344
