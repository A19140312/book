#工厂模式

###1. 简单工厂模式(静态工厂模式)
* 定义：由一个工厂对象决定创建出哪一种产品类的实例，被创建的实例通常都具有共同的父类。
* 模拟场景：店里买肉夹馍
* 代码：

```java
/**
 * 简单工厂模式
 */
public RoujiaMo creatRoujiaMo(String type) {
    RoujiaMo roujiaMo = null;
    switch (type) {
        case "Suan":
            roujiaMo = new SuanRoujiaMo();
            break;
        case "La":
            roujiaMo = new LaRoujiaMo();
            break;
        case "Tian":
            roujiaMo = new TianRoujiaMo();
            break;
        default:// 默认为酸肉夹馍
            roujiaMo = new SuanRoujiaMo();
            break;
    }
    return roujiaMo;
}

```


* 优点：简单工厂模式能够根据外界给定的信息，决定究竟应该创建哪个具体类的对象。明确区分了各自的职责和权力，有利于整个软件体系结构的优化。
* 缺点：很明显工厂类集中了所有实例的创建逻辑，容易违反开闭原则。


###2. 工厂方法模式
* 定义：定义一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法模式把类实例化的过程推迟到子类。
* 模拟场景：开分店
* 步骤：
    * 步骤1：创建**抽象工厂类**，定义具体工厂的公共接口； 
    * 步骤2：创建**抽象产品类** ，定义具体产品的公共接口； 
    * 步骤3：创建**具体产品类**（继承抽象产品类） & 定义生产的具体产品； 
    * 步骤4：创建**具体工厂类**（继承抽象工厂类），定义创建对应具体产品实例的方法； 
    * 步骤5：外界通过调用具体工厂类的方法，从而创建不同**具体产品类的实例**
* 代码：


```java
/**
 * 步骤1：创建**抽象工厂类**，定义具体工厂的公共接口；
 * 提供创建工厂类抽象方法
 */

public abstract class Factory{
    public abstract RoujiaMo Manufacture();
}
/**
 * 步骤2：创建**抽象产品类** ，定义具体产品的公共接口；
 * 提供创建肉夹馍店抽象方法
 */
public abstract class RoujiaMo {

    public abstract void sellRoujiaMo();

}

/**
 * 步骤3：创建**具体产品类**（继承抽象产品类） & 定义生产的具体产品；
 * 西安肉夹馍店   让分店自己去卖自己口味的肉夹馍
 */

public class XianRoujiaMo extends RoujiaMo {
    @Override
    public void sellRoujiaMo() {
        System.out.println("卖出了西安肉夹馍");
    }
}

/**
 * 步骤4：创建**具体工厂类**（继承抽象工厂类），定义创建对应具体产品实例的方法； 
 * 用来西安店生产自己店里的肉夹馍
 */

public class XianRoujiaMoFactory extends Factory{
     @Override
    public RoujiaMo Manufacture() {
        return new XianRoujiaMo();
    }
}
/**
 * 步骤5：外界通过调用具体工厂类的方法，从而创建不同具体产品类的实例
 */

public class FactoryPattern {
    public static void main(String[] args){
        //客户要西安肉夹馍
        RoujiaMo roujiaMo = new XianRoujiaMo();
        roujiaMo.Manufacture().sellRoujiaMo();
    }
}
```

* 优点：
    * 更符合开-闭原则 ，新增一种产品时，只需要增加相应的具体产品类和相应的工厂子类即可
    * 符合单一职责原则 每个具体工厂类只负责创建对应的产品
    * 不使用静态工厂方法，可以形成基于继承的等级结构。
* 缺点：
    * 添加新产品时，除了增加新产品类外，还要提供与之对应的具体工厂类，系统类的个数将成对增加，在一定程度上增加了系统的复杂度；同时，有更多的类需要编译和运行，会给系统带来一些额外的开销；
    * 由于考虑到系统的可扩展性，需要引入抽象层，在客户端代码中均使用抽象层进行定义，增加了系统的抽象性和理解难度，且在实现时可能需要用到DOM、反射等技术，增加了系统的实现难度。
    * 虽然保证了工厂方法内的对修改关闭，但对于使用工厂方法的类，如果要更换另外一种产品，仍然需要修改实例化的具体工厂类；
    * 一个具体工厂只能创建一种具体产品


###3. 抽象工厂模式
* 定义：提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类；具体的工厂负责实现具体的产品实例。
* **抽象工厂模式与工厂方法模式最大的区别**：**抽象工厂**中每个工厂可以创建多种类的产品；而**工厂方法**每个工厂只能创建一类
* 步骤：
    * 步骤1： 创建**抽象工厂类**，定义具体工厂的公共接口；
    * 步骤2： 创建**抽象产品族类** ，定义抽象产品的公共接口；
    * 步骤3： 创建**抽象产品类** （继承抽象产品族类），定义具体产品的公共接口；
    * 步骤4： 创建**具体产品类**（继承抽象产品类） & 定义生产的具体产品；
    * 步骤5：创建**具体工厂类**（继承抽象工厂类），定义创建对应具体产品实例的方法；
    * 步骤6：客户端通过实例化具体的工厂类，并调用其创建不同目标产品的方法创建不同具体产品类的实例
* 模拟场景：
    * 背景：小成有两间塑料加工厂（A厂仅生产容器类产品；B厂仅生产模具类产品）；随着客户需求的变化，A厂所在地的客户需要也模具类产品，B厂所在地的客户也需要容器类产品；
    * 冲突：没有资源（资金+租位）在当地分别开设多一家注塑分厂
    * 解决方案：在原有的两家塑料厂里增设生产需求的功能，即A厂能生产容器+模具产品；B厂间能生产模具+容器产品。
* 代码：


```java
//步骤1： 创建抽象工厂类，定义具体工厂的公共接口
abstract class Factory{
    public abstract Product ManufactureContainer();
    public abstract Product ManufactureMould();
}
//步骤2： 创建抽象产品族类 ，定义具体产品的公共接口；
abstract class Product{
    public abstract void Show();
}
```

```java

//步骤3： 创建抽象产品类 ，定义具体产品的公共接口；
//容器产品抽象类
abstract class ContainerProduct extends Product{
    @Override
    public abstract void Show();
}
//模具产品抽象类
abstract class MouldProduct extends Product{
    @Override
    public abstract void Show();
}

```

```java
//步骤4： 创建具体产品类（继承抽象产品类）， 定义生产的具体产品；
//容器产品A类
class ContainerProductA extends ContainerProduct{
    @Override
    public void Show() {
        System.out.println("生产出了容器产品A");
    }
}

//容器产品B类
class ContainerProductB extends ContainerProduct{
    @Override
    public void Show() {
        System.out.println("生产出了容器产品B");
    }
}

//模具产品A类
class MouldProductA extends MouldProduct{

    @Override
    public void Show() {
        System.out.println("生产出了模具产品A");
    }
}

//模具产品B类
class MouldProductB extends MouldProduct{

    @Override
    public void Show() {
        System.out.println("生产出了模具产品B");
    }
}
```


```java
//步骤5：创建具体工厂类（继承抽象工厂类），定义创建对应具体产品实例的方法；
//A厂 - 生产模具+容器产品
class FactoryA extends Factory{

    @Override
    public Product ManufactureContainer() {
        return new ContainerProductA();
    }

    @Override
    public Product ManufactureMould() {
        return new MouldProductA();
    }
}

//B厂 - 生产模具+容器产品
class FactoryB extends Factory{

    @Override
    public Product ManufactureContainer() {
        return new ContainerProductB();
    }

    @Override
    public Product ManufactureMould() {
        return new MouldProductB();
    }
}

```


```java
//步骤6：客户端通过实例化具体的工厂类，并调用其创建不同目标产品的方法创建不同具体产品类的实例
//生产工作流程
public class AbstractFactoryPattern {
    public static void main(String[] args){
        FactoryA mFactoryA = new FactoryA();
        FactoryB mFactoryB = new FactoryB();
        //A厂当地客户需要容器产品A
        mFactoryA.ManufactureContainer().Show();
        //A厂当地客户需要模具产品A
        mFactoryA.ManufactureMould().Show();

        //B厂当地客户需要容器产品B
        mFactoryB.ManufactureContainer().Show();
        //B厂当地客户需要模具产品B
        mFactoryB.ManufactureMould().Show();

    }
}
```
* 优点：降低耦合；更符合开-闭原则；符合单一职责原则；不使用静态工厂方法，可以形成基于继承的等级结构。
* 缺点：抽象工厂模式很难支持**新种类产品**的变化。这是因为抽象工厂接口中已经确定了可以被创建的产品集合，如果需要添加新产品，此时就必须去修改抽象工厂的接口，这样就涉及到抽象工厂类的以及所有子类的改变，这样也就**违背了“开发——封闭”原则**。




