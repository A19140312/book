###与java内存模型交互时的指南
*    使用synchronized或volatile来保护在多个线程之间共享的字段
*    将常量字段设置为final
*    不要从构造函数中泄露this

#重排序
指编译器和java虚拟机通过改变程序的处理顺序来优化程序。

*    单线程程序中我们无法判断是否进行了重排序。
*    在多线程程序中，有时会发生明显的重排序导致的运行错误。

#可见性
假设线程A将某值写入到了字段x中，而线程B读到了该值。我们称其为“线程A向x的写值对线程B是可见的”。这个性质就称为可见性。

*    在单线程中无需在意可见性，因为线程总是可以看见自己写入到字段中的值。    
*    在多线程中**必须**注意可见性。因为如果没有使用synchronized或volatile正确的进行同步，**线程A写入到字段中的值可能不会立即对线程B可见**。 

#共享内存与操作
![](/assets/WechatIMG1.jpeg)
##共享内存（堆内存）
是所有线程共享的存储空间，也被称为**堆内存**。
**实例**会被全部保存在共享内存中，所以**实例中的字段**也存在于共享内存中。
**数组的元素**也被保存在共享内存中。
也就是说，可以使用new在共享内存中分配存储空间。

**局部变量，方法的形参，catch语句块中编写的异常处理器的参数**保存在各个线程特有的栈中。
##synchronized
synchronized具有线程的互斥处理和同步处理两种功能。
*    **线程的互斥处理**：线程会在synchronized开始时获取锁（lock）在synchronized终止时释放锁（unlock）.只有一个线程能获取实例的锁。
*    **同步处理**：某个线程在进行unlock操作前进行的所有写入操作对进行lock操作对线程都是可见对。

##volatile
volatile具有**同步处理**和对long和double的原子操作。
*    **同步处理**：某个线程对volatile字段进行的写操作的结果对其他线程立即可见。换而言之，对volatile字段的写入处理并不会被缓存起来。此外，**在向volatile字段读取和写入前后不会进行重排序。**volatile不进行线程的互斥处理。
访问volatile字段与synchronized的处理耗费的时间几乎相同。
*    **对long和double的原子操作**：long和double的字段只要是volatile字段，就可以保证赋值操作的原子性。

##final
使用final关键字声明的字段只能被初始化一次。final字段的初始化只能在**声明字段时**或在**构造函数**中进行。java内存模型确保final字段在构造函数执行结束后可以正确的被看到，这样就不需要通过synchronized和volatile进行同步了。
*ConcurentHashMap类使用final和volatile特性实现了无阻塞的Map。