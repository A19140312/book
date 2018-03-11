#单例模式
##1. 定义
确保某个类只有一个实例，能自行实例化并向整个系统提供这个实例。
##2. 应用场景
    * 资源共享的情况下，避免由于资源操作时导致的性能或损耗等。如上述中的日志文件，应用配置。 
    * 控制资源的情况下，方便资源之间的互相通信。如线程池等。 
    
##3. 实现
###3.1 饿汉单例模式


```java
/**
* 饿汉单例模式
**/
public class EhanSingleton {
	//static final单例对象，类加载的时候就初始化
	private static final EhanSingleton instance = new EhanSingleton();

	//私有构造方法，使得外界不能直接new
	private EhanSingleton(){}

	//公有静态方法，对外提供获取单例接口
	public static EhanSingleton getInstance(){
		return instance;
	}
}

```
* 优点：**线程安全**。因为在加载这个类的时候，就实例化了instance。当getInstatnce方法被调用时，得到的永远是类加载时初始化的对象（反序列化的情况除外）。
* 缺点：如果有大量的类都采用了饿汉单例模式，那么在类加载的阶段，会初始化很多暂时还没有用到的对象，这样肯定会**浪费内存，影响性能**

###3.2 懒汉单例模式


```java
/**
 * 懒汉单例模式
 */
public class LanhanSingleton {
	//static单例对象，类加载的时候为null
	private static LanhanSingleton instance;

	//私有构造方法，使得外界不能直接new
	private LanhanSingleton(){}

	//增加synchronized关键字，该方法为同步方法，保证多线程单例对象唯一
	public static synchronized LanhanSingleton getInstance(){
		if(instance == null){
			instance = new LanhanSingleton();
		}
		return instance;
	}
}

```
* 优点：单例对象在第一次调用才被实例化，有效节省内存，并保证了线程安全。
* 缺点：同步是针对方法的，以后每次调用getInstance时（就算intance已经被实例化了），也会进行同步，造成了不必要的同步开销。不推荐使用这种方式。

###3.3 Double CheckLock(DCL)单例模式



```java
/**
 * 双重检测单例
 */
public class DCLSingleton {
	//被volatile修饰的变量的值，将不会被本地线程缓存
	//所有对该变量的读写都是直接操作共享内存,从而确保多个线程能正确的处理该变量。
	private volatile static DCLSingleton instance;
	
	private DCLSingleton(){}

	public static DCLSingleton getInstance() {
		if(instance == null){
			synchronized (DCLSingleton.class){
				//当第一次调用getInstance方法时，即instance为空时，同步操作，保证多线程实例唯一
				//当以后调用getInstance方法时，即instance不为空时，不进入同步代码块，减少了不必要的同步开销
				if(instance == null){
					instance = new DCLSingleton();
				}
			}
		}
		return instance;
	}
}

```
* 优点：
	* 第一次执行getInstance时instance才被实例化，节省内存；
	* 多线程情况下，基本安全；
	* 并且在instance实例化以后，再次调用getInstance时，不会有同步消耗。
* 缺点：
	* jdk1.5以下，有可能DCL失效；
	* Java内存模型影响导致失效；
	* jdk1.5以后，使用volatile关键字，虽然能解决DCL失效问题，但是会影响部分性能。

###3.4 静态内部类单例模式

```java
/**
 * 静态内部类单例
 */
public class StaticSingleton {
	//私有的构造方法，防止new
	private StaticSingleton(){}

	/**
	 * 静态内部类
	 * 第一次加载内部类的时候，实例化单例对象
	 */
	private static class StaticSingletonHolder{
		private static final StaticSingleton instance = new StaticSingleton();
	}

	public static StaticSingleton getInstance(){
		return StaticSingletonHolder.instance;
	}
}

```
* 优点：第一次加载StaticClassSingleton类时，并不会实例化instance，只有第一次调用getInstance方法时，Java虚拟机才会去加载StaticClassSingletonHolder类，继而实例化instance。
* 缺点：第一次加载时反应不够快 

###总结
|  名称 | 优点  | 缺点   | 备注   |
|  ----  |  ---- |  ----  |  ----  | 
| 饿汉模式  | 线程安全 | 内存消耗太大 |  |
| 懒汉模式  | 线程安全 | 同步方法消耗比较大 |  | 
| DCL模式 | 线程安全，节省内存 | jdk版本受限、高并发会导致DCL失效 | **推荐使用**  |
| 静态内部类模式| 线程安全、节省内存 | 实现比较麻烦  | **推荐使用** |







