#Spring
###什么是Spring？
Spring是一个开源的Java EE开发框架。是一个为Java应用程序的开发提供了综合、广泛的基础性支持的Java平台。
###Spring的优点
* 轻量级：Spring在大小和透明性方面绝对属于轻量级的，基础版本的Spring框架大约只有2MB。

* 控制反转(IOC)：Spring使用控制反转技术实现了松耦合。依赖被注入到对象，而不是创建或寻找依赖对象。

* 面向切面编程(AOP)： Spring支持面向切面编程，同时把应用的业务逻辑与系统的服务分离开来。

* 容器：Spring包含并管理应用程序对象的配置及生命周期。

* MVC框架：Spring的web框架是一个设计优良的web MVC框架，很好的取代了一些web框架。

* 事务管理：Spring对下至本地业务上至全局业务(JAT)提供了统一的事务管理接口。

* 异常处理：Spring提供一个方便的API将特定技术的异常(由JDBC, Hibernate, 或JDO抛出)转化为一致的、Unchecked异常。

###Spring框架有哪些模块？
![](/assets/20160906195535830.png)
###什么是控制反转(IOC)？什么是依赖注入(DI)？
* IOC：就是由容器控制程序之间的（依赖）关系，而非传统实现中，由程序代码直接操控。（依赖）控制权由应用代码中转到了外部容器，控制权的转移，是所谓反转。
* DI：所谓依赖注入，即组件之间的依赖关系由容器在运行期决定，形象的来说，即由容器动态的将某种依赖关系注入到组件之中。

###Spring的注解
* **@Autowired：自动装配**；@Autowired默认按类型匹配的方式，在容器查找匹配的Bean，当有且仅有一个匹配的Bean时，Spring将其注入@Autowired标注的变量中。
* **@Resource：**与@Autowired相似。
    * @Autowired默认按照byType方式进行bean匹配，@Resource默认按照byName方式进行bean匹配
    * @Autowired是Spring的注解，@Resource是J2EE的注解，这个看一下导入注解的时候这两个注解的包名就一清二楚了    
* **@Service**：对应的是业务层Bean；
* **@Controller**：对应表现层的Bean，也就是Action
* **@Repository**：用于标注数据访问组件，即DAO组件。
* **@Component**：泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。

###Spring依赖注入(DI)的3中方法
* 1.构造器注入

```java
public class xx {
    private Manager manage;
    public xx(Manager manage){
        this.manage= manage;
    }
}
```
* 2.set方法注入

```java
public class xx {
    private Manager manage;
    public void setManager(Manager manage){
        this.manage= manage;
    }
}

```
* 3.接口注入


```java
public interface Manager{
     public void manage(Business business);
}
public class xx implements Manager{
    private Business business;
    
    @Override
    public void manage(Business business){
        this.business = business;
    }
}
```




