# MulticsDevOps
## 服务部署笔记 & 学习笔记 & 源代码-使用教程。
### Docker&kubernetes
* [给Docker配置官方国内加速镜像](https://github.com/MulticsYin/MulticsDevOps/blob/master/Docker/%E7%BB%99Docker%E9%85%8D%E7%BD%AE%E5%AE%98%E6%96%B9%E5%9B%BD%E5%86%85%E5%8A%A0%E9%80%9F%E9%95%9C%E5%83%8F.md#给docker配置官方国内加速镜像)  
出于网路原因，国内访问 Docker Hub 有时会遇到困难，下载官方镜像更是望穿秋水。该教程解决这个问题，配置国内镜像加速器，为镜像下载加速。  

* [RunC 源码解读](https://github.com/MulticsYin/MulticsDevOps/blob/master/RunC/runc00.md#runc-源码解读)

### 网络编程
* [高性能网络编程](http://taohui.pub/?cat=9)[陶辉笔记]  
掌握高性能网络编程，涉及到对网络、操作系统协议栈、进程与线程、常见的网络组件等知识点，需要有丰富的项目开发经验，能够权衡服务器运行效率与项目开发效率。 

* [高性能网络编程1 - ACCEPT建立连接](https://github.com/MulticsYin/MulticsDevOps/blob/master/NetworkProgram/NetworkProgram00.md#高性能网络编程1---accept建立连接)  
高性能网络编程系列文章前言，以及阐述`ACCEPT建立连接`，为后文打下基础。  

* [高性能网络编程2 - TCP消息的发送](https://github.com/MulticsYin/MulticsDevOps/blob/master/NetworkProgram/NetworkProgram01.md#高性能网络编程2---tcp消息的发送)  
发送方法成功返回时，能保证TCP另一端的主机接收到吗？能保证数据已经发送到网络上了吗？套接字为阻塞或者非阻塞时，发送方法做的事情有何不同？  

* [高性能网络编程3 - TCP消息的接收](https://github.com/MulticsYin/MulticsDevOps/blob/master/NetworkProgram/NetworkProgram02.md#高性能网络编程3---tcp消息的接收)  
这篇文章将试图说明应用程序如何接收网络上发送过来的TCP消息流，由于篇幅所限，暂时忽略ACK报文的回复和接收窗口的滑动。  

* [高性能网络编程4 - TCP连接的关闭](https://github.com/MulticsYin/MulticsDevOps/blob/master/NetworkProgram/NetworkProgram03.md#高性能网络编程4---tcp连接的关闭)  
本文首先说说多线程多进程关闭连接的区别；再用一幅流程图谈谈close；最后用一幅流程图说说shutdown。  

* [高性能网络编程5 - IO复用与并发编程](https://github.com/MulticsYin/MulticsDevOps/blob/master/NetworkProgram/NetworkProgram04.md#高性能网络编程5---io复用与并发编程)  
多路复用可以同时处理多个连接！它也可能“等待”，所以它也会导致线程睡眠，然而这不要紧，因为它一对多、它可以监控所有连接。这样，没有那么多个线程都在争抢处理“等待消息准备好”阶段，整个世界终于清净了！  

* [高性能网络编程6 - REACTOR反应堆与定时器管理](https://github.com/MulticsYin/MulticsDevOps/blob/master/NetworkProgram/NetworkProgram05.md#高性能网络编程6---reactor反应堆与定时器管理)  
反应堆开发模型被绝大多数高性能服务器所选择，上一篇所介绍的IO多路复用是它的实现基础。定时触发功能通常是服务器必备组件，反应堆模型往往还不得不将定时器的管理囊括在内。本篇将介绍反应堆模型的特点和用法。  

* [大小字节序理解和鉴定系统字节序方法](https://github.com/MulticsYin/MulticsDevOps/blob/master/NetworkProgram/site.md#大小字节序理解和鉴定系统字节序方法)[转]  
计算机硬件有两种储存数据的方式：大端字节序（big endian）和小端字节序（little endian）。

### Git&GitHub
* [Ubuntu16.04下Git&Gitosis服务器安装与配置](https://github.com/MulticsYin/MulticsDevOps/blob/master/Git-Github/Ubuntu16.04%E4%B8%8BGit&Gitosis%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%89%E8%A3%85%E4%B8%8E%E9%85%8D%E7%BD%AE.md#ubuntu1604下gitgitosis服务器安装与配置)  
在实验室或者公司内部搭建一个局域网Git服务器(使用git和gitosis)。

### Web 相关
* [如何免费的让网站启用HTTPS](https://github.com/MulticsYin/MulticsDevOps/blob/master/Web/free_HTTPS.md)[转]  
使用 `Let’s Encrypt` 免费的解决方案。`Let’s Encrypt` 是一个于2015年推出的数字证书认证机构，将通过旨在消除当前手动创建和安装证书的复杂过程的自动化流程，为安全网站提供免费的SSL/TLS证书。  

* [负载均衡层技术](http://blog.csdn.net/column/details/load-balancing.html)（CSDN博客链接）  
主流负载均衡技术在HTTP系统中的应用，详细介绍了负载均衡系统涉及的主要设计思想，重点讲解了Nginx、LVS、Keepalived技术原理和使用方式。

* [负载均衡层技术(补充) - HAProxy](https://github.com/MulticsYin/MulticsDevOps/blob/master/Web/LoadBalancing.md)  
HAProxy是基于TCP四层和HTTP七层的开源的第三方应用负载均衡软件。具有高可靠性、高稳定性、高并发处理能力、透明代理和支持ACL功能等特点。HAProxy是一个功能强大且优秀的负载均衡集群解决方案。

* [Nginx配置文件nginx.conf详解](https://github.com/MulticsYin/MulticsDevOps/blob/master/Web/nginx_conf.md)  
Nginx("engine x")是一款是由俄罗斯的程序设计师Igor Sysoev所开发高性能的 Web和 反向代理 服务器，也是一个 IMAP/POP3/SMTP 代理服务器。
在高连接并发的情况下，Nginx是Apache服务器不错的替代品。  

* [HTTPS简述](https://github.com/MulticsYin/MulticsDevOps/blob/master/Web/https.md)  
HTTPS（全称：Hyper Text Transfer Protocol over Secure Socket Layer），是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。 它是一个URI scheme（抽象标识符体系），句法类同http:体系。用于安全的HTTP数据传输。https:URL表明它使用了HTTP，但HTTPS存在不同于HTTP的默认端口及一个加密/身份验证层（在HTTP与TCP之间）。  

* [Web系统的架构分层](https://github.com/MulticsYin/MulticsDevOps/blob/master/Web/WebSystem.md)  
大型动态应用系统平台主要是针对于大流量、高并发网站建立的底层系统架构。大型网站的运行需要一个可靠、安全、可扩展、易维护的应用系统平台做为支撑，以保证网站应用的平稳运行。该文章简述Web系统的架构分层，并说明每个模块。  

### 分布式系统相关组件
* [ZooKeeper](https://github.com/MulticsYin/MulticsDevOps/blob/master/DistributedSystems/Zookeeper.md)  
ZooKeeper是源代码开放的分布式协调服务，由雅虎创建，是Google的开源实现。ZooKeeper是一个高性能的分布式数据一致性解决方案，他将那些复杂的、容易出错的分布式一致性服务封装起来，构成一个高效可靠的原语集，并提供一系列简单易用的接口给用户使用。  

* [Kafka中文教程](http://www.orchome.com/kafka/index)（OrcHome链接）  
Kafka是由LinkedIn开发的一个分布式的消息系统，使用Scala编写，它以可水平扩展和高吞吐率而被广泛使用。  

* [Zookeeper&Kafka集群配置](https://github.com/MulticsYin/MulticsDevOps/blob/master/DistributedSystems/Zookeeper&Kafka.md#zookeeperkafka集群配置)  
该教程在虚拟机（VMware）中虚拟三台Linux服务器，构建一个Zookeeper&Kafka集群  

### 微服务 - Micro service
* [OAuth2.0 鉴权机制](https://github.com/MulticsYin/MulticsDevOps/blob/master/MicroService/OAuth2.0.md#oauth20-鉴权机制)  
OAuth在"客户端"与"服务提供商"之间，设置了一个授权层（authorization layer）。"客户端"不能直接登录"服务提供商"，只能登录授权层，以此将用户与客户端区分开来。"客户端"登录授权层所用的令牌（token），与用户的密码不同。用户可以在登录的时候，指定授权层令牌的权限范围和有效期。  

* [认证鉴权与API权限控制在微服务架构中的设计与实现](http://blueskykong.com/categories/Security/)(转载)  
系统微服务化后，原有的单体应用是基于Session的安全权限方式，不能满足现有的微服务架构的认证与鉴权需求。微服务架构下，一个应用会被拆分成若干个微应用，每个微应用都需要对访问进行鉴权，每个微应用都需要明确当前访问用户以及其权限。尤其当访问来源不只是浏览器，还包括其他服务的调用时，单体应用架构下的鉴权方式就不是特别合适了。在微服务架构下，要考虑外部应用接入的场景、用户–服务的鉴权、服务–服务的鉴权等多种鉴权场景。  
比如用户A访问User Service，A如果未登录，则首先需要登录，请求获取授权token。获取token之后，A将携带着token去请求访问某个文件，这样就需要对A的身份进行校验，并且A可以访问该文件。  
为了适应架构的变化、需求的变化，auth权限模块被单独出来作为一个基础的微服务系统，为其他业务service提供服务。

### 运维相关
* [Shell 编程](https://github.com/MulticsYin/MulticsDevOps/blob/master/Ops/shell.md#综合案例)  
Shell脚本入门指南
* MySQL
[MySQL创建用户与授权](https://github.com/MulticsYin/MulticsDevOps/blob/master/Ops/MYSQL/user.md)  

### vim
* [vim配置](https://github.com/MulticsYin/MulticsDevOps/blob/master/Vim/vimCongig.md#vim配置)  
Vim 是我最喜欢的代码编辑工具，共享下我的Vim配置，有效果图，欢迎提建议。



## License

MulticsDevOps source code is licensed under the 
[Apache Licence, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)
