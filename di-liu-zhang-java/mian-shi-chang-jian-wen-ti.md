#面试常见问题
###Integer通过==比较

```java

Integer a=10;
Integer b=10;​
Integer c=new Integer(10);
Integer d=new Integer(10);
System.out.println(a==b);//true
System.out.println(c==d);//false﻿​

```
**== 比较的是对象的引用 当且仅当比较的两个引用指向同一对象才返回true
** 
Integer i = XXX 看看Integer 的源代码就知道了, 其实就是Integer 把-128-127(一个字节的二进制补码) 之间的每个值都建立了一个对应的Integer 对象, 类似一个缓存. 由于Integer 是不可变类, 因此这些缓存的Integer 对象可以安全的重复使用. 
###java中的四种引用及其应用场景是什么？
*    强引用: 通常我们使用new操作符创建一个对象时所返回的引用即为强引用
*    软引用: 若一个对象只能通过软引用到达，那么这个对象在内存不足时会被回收，可用于图片缓存中，内存不足时系统会自动回收不再使用的Bitmap
*    弱引用: 若一个对象只能通过弱引用到达，那么它就会被回收（即使内存充足），同样可用于图片缓存中，这时候只要Bitmap不再使用就会被回收
*    虚引用: 虚引用是Java中最“弱”的引用，通过它甚至无法获取被引用的对象，它存在的唯一作用就是当它指向的对象回收时，它本身会被加入到引用队列中，这样我们可以知道它指向的对象何时被销毁。

###ArrayList, LinkedList, Vector的区别是什么？
*    ArrayList: 内部采用数组存储元素，支持高效随机访问，支持动态调整大小
*    LinkedList: 内部采用链表来存储元素，支持快速插入/删除元素，但不支持高效地随机访问
*    Vector: 可以看作线程安全版的ArrayList

###类的加载顺序
*    父类静态代码块(包括静态初始化块，静态属性，但不包括静态方法)
*    子类静态代码块(包括静态初始化块，静态属性，但不包括静态方法 )
*    父类非静态代码块( 包括非静态初始化块，非静态属性 )
*    父类构造函数
*    子类非静态代码块 ( 包括非静态初始化块，非静态属性 )
*    子类构造函数

###static和final的区别和用途
* static：
    * 修饰变量：静态变量随着类加载时被完成初始化。内存中只有一个，所有类共享静态变量。
    * 修饰方法：在类的加载时就存在，不依赖任何实例，staitc方法必须实现，不能用abstract修饰
    * 修饰代码块：在类加载完之后，就会执行代码块之中的内容
* final
    * 修饰变量：
        * 编译期常量：类加载的过程中完成初始化，编译后带入到任何计算式只能是基本类型
        * 运行时常量：基本数据类型或引用数据类型，引用不可变但引用对象内容不可变。
    * 修饰方法：不能被继承，不能被子类修改。
    * 修饰类：不能被继承
    * 修饰形参：形参不可变
###ArrayList扩容机制
* 在JDK1.7中，如果通过无参构造的话，**初始数组容量为0**，当真正对数组进行添加时，才真正分配容量(默认分配10)。每次按照1.5倍（位运算）的比率通过copeOf的方式扩容。 
* 在JKD1.6中，如果通过无参构造的话，**初始数组容量为10**.每次通过copeOf的方式扩容后容量为原来的1.5倍加1.