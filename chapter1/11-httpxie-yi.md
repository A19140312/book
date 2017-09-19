# 1.1  HTTP协议

###HTTP协议
Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协


###HTTP的特性

* HTTP构建于TCP/IP协议之上，默认端口号是80
* HTTP是无连接无状态的
    + 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
    + 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

##HTTP请求与响应
###客户端Request：
请求行，请求头，空行，请求数据四个部分。
![](http://upload-images.jianshu.io/upload_images/2964446-fdfb1a8fce8de946.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=5984001)

###服务端Response：
状态行、消息报头、空行和响应正文。
![](http://upload-images.jianshu.io/upload_images/2964446-1c4cab46f270d8ee.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=5984001)
#HTTP状态码
状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:
