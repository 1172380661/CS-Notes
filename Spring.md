## Spring中的事务隔离级别 ##
- ISOLATION_DEFAULT						使用数据库的默认隔离级别
- ISOLATION_READ_UNCOMMITED				未提交读
- ISOLATION_READ_COMMITED				提交读
- ISOLATION_REPEATABLE_READ				可重复读
- ISOLATION_SERIALIZABLE				序列化
## Spring中的事务传播特性 ##
假设A方法中调用了B  

- PROPAGATION_REQUIRED：如果A运行在一个事务中将B也加入到这个事务里，如果没有则B会为自己分配成一个事务。					
- PROPAGATION_REQUIRED_NEW：无论如何B都会新建一个事务，这个事务和外层的事务毫无关系。
- PROPAGATION_SUPPORTS：A如果有事务则B加入到这个事务中，如果没有B也不新建事务。
- PROPAGATION_MANDATORY：A如果有事务则B加入到这个事务中，如果抛出异常。
- PROPAGATION_NOT_SUPPORTED：A如果有事务，则先将其挂起。
- PROPAGATION_NEVER：A如果有事务，抛出异常。
- PROPAGATION_NESTED：和PROPAGATION_REQUIRED_NEW类似，区别在于B事务和A事务有关联，A可以根据B事务是否回滚来决定接下来的程序执行逻辑。