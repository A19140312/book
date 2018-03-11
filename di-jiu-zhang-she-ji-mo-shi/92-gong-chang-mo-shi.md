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
* 代码：

```java

```


* 优点：
* 缺点：

###3. 抽象工厂模式