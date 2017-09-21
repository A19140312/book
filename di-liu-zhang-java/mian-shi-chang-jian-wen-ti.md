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