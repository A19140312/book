#String
*    java.lang.String类中并没有修改字符串内容的方法，是immutable类(不可变类)。也就是说，**String实例所表示的字符串绝对不会发生变化。也就无需声明sychronized。**
*    字符串和实例表达式通过运算符“+”连接时，程序会自动调用实例表达式的toString()方法。(Java规范)

####StringBuffer
*    StringBuffer表示的字符串能够随意改写，是mutable类(可变类)。改写时需要妥善使用synchronized。
*    String的引用的速度快，而频繁修改字符串内容StringBuffer的速度快。