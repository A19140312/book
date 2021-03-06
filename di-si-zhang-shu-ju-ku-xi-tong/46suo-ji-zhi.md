#4.6锁机制

#从数据库系统的角度来看分为：
##1.共享锁（S）
共享锁又称读锁，是读取操作创建的锁。其他用户可以并发读取数据，但任何事务都不能对数据进行修改（获取数据上的排他锁），直到已释放所有共享锁。

##2.更新锁（U）
更新锁可以防止通常形式的死锁。一般更新模式由一个事务组成，此事务读取记录，获取资源（页或行）的共享锁，然后修改行，此操作要求锁转换为排它锁。

##3.排他锁（X）
排它锁可以防止并发事务对资源进行访问。其它事务不能读取或修改排它锁锁定的数据。

##4.意向锁

* 意向共享锁（IS）：表示事务准备给数据行加入共享锁，也就是说一个数据行加共享锁前必须先取得该表的IS锁

* 意向排他锁（IX）：类似上面，表示事务准备给数据行加入排他锁，说明事务在一个数据行加排他锁前必须先取得该表的IX锁。

* 意向共享排他锁（SIX）：对一个数据对象加 SIX锁，表示对它加 S锁，再加IX锁，即 SIX=S+IX。例如对某个表加 SIX锁，则表示该事务要读整个表（所以要对该表加 S锁），同时会更新个别元组（所以要对该表加 IX锁）。

意向锁就是说在屋（比如代表一个表）门口设置一个标识，说明屋子里有人（比如代表某些记录）被锁住了。另一个人想知道屋子里是否有人被锁，不用进屋子里一个一个的去查，直接看门口标识就行了。

当一个表中的某一行被加上排他锁后，该表就不能再被加表锁。数据库程序如何知道该表不能被加表锁？一种方式是逐条的判断该表的每一条记录是否已经有排他锁，另一种方式是直接在表这一层级检查表本身是否有意向锁，不需要逐条判断。显然后者效率高。

#如何提高并发效率
##1.悲观锁
悲观锁，正如其名，它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度(悲观)，因此，在整个数据处理过程中，将数据处于锁定状态。
* 悲观锁的流程

    在对任意记录进行修改前，先尝试为该记录加上排他锁（exclusive locking）。
    如果加锁失败，说明该记录正在被修改，那么当前查询可能要等待或者抛出异常。 具体响应方式由开发者根据实际需要决定。
    如果成功加锁，那么就可以对记录做修改，事务完成后就会解锁了。
    其间如果有其他对该记录做修改或加排他锁的操作，都会等待我们解锁或直接抛出异常。
* 优点与不足

    悲观并发控制实际上是“先取锁再访问”的保守策略，为数据处理的安全提供了保证。但是在效率方面，处理加锁的机制会让数据库产生额外的开销，还有增加产生死锁的机会；另外，在只读型事务处理中由于不会产生冲突，也没必要使用锁，这样做只能增加系统负载；还有会降低了并行性，一个事务如果锁定了某行数据，其他事务就必须等待该事务处理完才可以处理那行数    
    
##2.乐观锁
乐观锁 相对悲观锁而言，乐观锁假设认为数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让返回用户错误的信息，让用户决定如何去做。
相对于悲观锁，在对数据库进行处理的时候，乐观锁并不会使用数据库提供的锁机制。一般的实现乐观锁的方式就是记录数据版本。
* 实现方式
    * 对记录加版本号.
        * 在数据初始化时指定一个版本号，每次对数据的更新操作都对版本号执行+1操作。并判断当前版本号是不是该数据的最新的版本号。
    * 对记录加时间戳.
        * 在数据初始化时使用时间戳（timestamp），在更新提交的时候检查当前数据库中数据的时间戳和自己更新前取到的时间戳进行对比，如果一致则OK。
    * 对将要更新的数据进行提前读取、事后对比。
    
#锁的粒度
锁的粒度就是指锁的生效范围，就是说是行锁，还是页锁，还是整表锁. 锁的粒度同样既可以由数据库自动管理，也可以通过手工指定hint来管理。
#锁的兼容关系
| Requested mode | IS  | S   | U   | IX  | SIX | X  |
|  ----  |  ---- |  ----  |  ----  |  ----  | ---- | ----|
| 意向共享锁 (IS)  | Yes | Yes | Yes | Yes | Yes | No |
| 共享锁 (S)  | Yes | Yes | Yes | No  | No  | No |
| 更新锁 (U) | Yes | Yes | No  | No  | No  | No |
| 意向排他锁 (IX)| Yes | No  | No  | Yes | No  | No |
| 意向共享排他锁 (SIX) | Yes | No  | No  | No  | No  | No |
| 排他锁 (X)  | No  | No  | No  | No  | No  | No |

#锁与事务的关系
![锁与事务的关系
](http://static.oschina.net/uploads/img/201207/09074335_5YM8.gif)

#封锁协议
##一级封锁协议
一级封锁协议对应**READ-UNCOMMITTED** 隔离级别，本质是在事务A中修改完数据M后，立刻对这个数据M加上共享锁(S锁)[当事务A继续修改数据M的时候，先释放掉S锁，再修改数据，再加上S锁]，根据S锁的特性，事务B可以读到事务A修改后的数据(无论事务A是否提交，因为是共享锁，随时随地都能查到数据A修改后的结果)，事务B不能去修改数据M，直到事务A提交，释放掉S锁。

* 缺点：丢失更新。脏读。不可重复读。幻读。

##二级封锁协议
二级封锁协议对应**READ-COMMITTED**隔离级别，本质是事务A在修改数据M后立刻加X锁，事务B不能修改数据M，同时不能查询到最新的数据M(避免脏读)，查询到的数据M是上一个版本(Innodb MVCC快照)的。
* 优点：避免脏读。
* 缺点：丢失更新。不可重复读。幻读。


##三级封锁协议
三级封锁协议对应**REPEATABLE-READ**隔离级别,本质是二级封锁协议基础上，对读到的数据M瞬间加上共享锁，直到事务结束才释放（保证了其他事务没办法修改该数据），这个级别是MySql 5.5 默认的隔离级别。

* 优点：避免脏读。避免不可重复读。
* 缺点：丢失更新。幻读。

##最强封锁协议
最强封锁协议对应**Serialization**隔离级别，本质是从MVCC并发控制退化到基于锁的并发控制，对事务中所有读取操作加S锁，写操作加X锁，这样可以避免脏读，不可重复读，幻读，更新丢失，开销也最大，会造成读写冲突，并发程度也最低。

    
    
