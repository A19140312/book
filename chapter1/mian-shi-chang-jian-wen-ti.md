#面试常见问题

### 请简述TCP\UDP的区别
 
* TCP面向连接，UDP面向非连接即发送数据前不需要建立链接
* TCP提供可靠的服务（数据传输），UDP无法保证
* TCP面向字节流，UDP面向报文
* TCP数据传输慢，UDP数据传输快

### TCP与UDP分别应用在什么方面
* TCP:对效率要求低，对准确性要求较高 (如文件传输、重要状态的更新等)
* UDP:对效率要求高，对准确性要求较低 (如视频传输、实时通信等)。

###TCP和UDP分别对应协议(应用层)
* TCP: STMP, TELNET, HTTP, FTP
* UDP: DNS,TFTP,RIP,DHCP,SNMP

###TCP三次握手的缺陷
* SYN- Flood攻击: 通过向网络服务所在端口发送大量的伪造源地址的攻击报文，就可能造成目标服务器中的半开连接队列被占满，从而阻止其他合法用户进行访问。
* 防范:
 * 无效连接监视释放 这种方法不停的监视系统中半开连接和不活动连接，当达到一定阈值时拆除这些连接，释放系统资源。这种绝对公平的方法往往也会将正常的连接的请求也会被释放掉，”伤敌一千，自损八百“
 * 延缓TCB分配方法
 
###端口及对应的服务
![常见端口对应服务](/assets/v2-e584c505e895441d7b52c8f3c02c9770_r.png)

###IP地址分为哪几类？简单说一下各个分类
![IP地址](/assets/v2-7438cb1ba454ffe278f5c2310e69f3aa_b.png)
###说一说OSI七层模型
![OSI七层模型](http://images2015.cnblogs.com/blog/705728/201604/705728-20160424234824085-667046040.png)
###为什么四次挥手，主动方要等待２MSL后才关闭连接
**保证TCP协议的全双工连接能够可靠关闭．** 主要为了确保对方能受到ACK信息. 如果Client直接CLOSED了，那么由于IP协议的不可靠性或者是其它网络原因，导致Server没有收到Client最后回复的ACK。那么Server就会在超时之后继续发送FIN，此时由于Client已经CLOSED了，就找不到与重发的FIN对应的连接，最后Server就会收到RST而不是ACK，Server就会以为是连接错误把问题报告给高层。所以，Client不是直接进入CLOSED，而是要保持2MSL,如果在这个时间内又收到了server的关闭请求时可以进行重传，否则说明server已经受到确认包则可以关闭.
_MSL(报文最大生存时间)_

##为什么要三次握手？
在《计算机网络》一书中其中有提到，三次握手的目的是“**为了防止已经失效的连接请求报文段突然又传到服务端，因而产生错误**”，这种情况是：一端(client)A发出去的第一个连接请求报文并没有丢失，而是因为某些未知的原因在某个网络节点上发生滞留，导致延迟到连接释放以后的某个时间才到达另一端(server)B。本来这是一个早已失效的报文段，但是B收到此失效的报文之后，会误认为是A再次发出的一个新的连接请求，于是B端就向A又发出确认报文，表示同意建立连接。如果不采用“三次握手”，那么只要B端发出确认报文就会认为新的连接已经建立了，但是A端并没有发出建立连接的请求，因此不会去向B端发送数据，B端没有收到数据就会一直等待，这样B端就会白白浪费掉很多资源。如果采用“三次握手”的话就不会出现这种情况，B端收到一个过时失效的报文段之后，向A端发出确认，此时A并没有要求建立连接，所以就不会向B端发送确认，这个时候B端也能够知道连接没有建立。

问题的本质是，信道是不可靠的，但是我们要建立可靠的连接发送可靠的数据，也就是数据传输是需要可靠的。在这个时候**三次握手是一个理论上的最小值**，并不是说是tcp协议要求的，而是**为了满足在不可靠的信道上传输可靠的数据**所要求的。
