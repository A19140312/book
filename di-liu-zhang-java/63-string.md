#String
*    java.lang.String类中并没有修改字符串内容的方法，是immutable类(不可变类)。也就是说，**String实例所表示的字符串绝对不会发生变化。也就无需声明sychronized。**
*    字符串和实例表达式通过运算符“+”连接时，程序会自动调用实例表达式的toString()方法。(Java规范)

###StringBuffer(线程安全)
*    StringBuffer表示的字符串能够随意改写，是mutable类(可变类)。改写时需要妥善使用synchronized。所以StringBuffer的方法中都要加锁。
*    String的引用的速度快，而频繁修改字符串内容StringBuffer的速度快。

###StringBuilder(非线程安全)
* 速度 ： StringBuilder > StringBuffer

###String中equals的实现

```java

public boolean equals(Object anObject) {
    /*若当前对象和比较多对象是同一对象*/
    if (this == anObject) {
        return true;
    }
    /*若当前传入的对象是String类型*/
    if (anObject instanceof String) {
        String anotherString = (String) anObject;
        int n = value.length;
        /*比较两个字符串长度*/
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            /*长度相同比较数组中每一位*/
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

