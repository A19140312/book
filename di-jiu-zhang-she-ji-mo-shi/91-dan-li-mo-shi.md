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
package singleton;

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

```






