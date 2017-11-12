# 负载均衡 -- HAProxy
该教程为负载均衡系列的 `HAProxy` 模块

## HAProxy是什么

HAProxy(High Available Proxy)是基于四层和七层的高可用负载均衡代理服务器，配置简单、支持多达上万条并发请求。HAProxy的运行模型使得它集成到现在的架构上非常容易和无风险，并且它还免去了将web服务器公开到网络上的风险，很多web站点都用HAProxy来作为七层负载均衡解决方案。

## HAProxy工作原理

HAProxy由前端（frontend）和后端（backend），前端和后端都可以有多个。也可以只有一个listen块来同时实现前端和后端。这里主要讲一下frontend和backend工作模式。

前端（frontend）区域可以根据HTTP请求的header信息来定义一些规则，然后将符合某规则的请求转发到相应后端（backend）进行处理。

## HAProxy配置文件

包含前端和后端的配置文件示例：

```
global
    daemon
    maxconn 500
    ulimit-n 8000
    user daemon
    group deamon
    chroot /var/empty
    pidfile /var/run/haproxy.pid
    log localhost local0 notice

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

frontend http-front
    bind *:80
    option httpclose
    maxconn 100

    reqirep ^Host:\\ www.abc.com Host:\\ abc.com

    acl host_abc_com hdr(host)      -i abc.com
    acl host_cn      hdr_end(host)  -i .cn
    acl host_xyz     hdr_beg(host)  -i xzy.
    acl url_xxx      url_reg        -i ^/xxx

    use_backend host-abc-com if host_abc_com
    use_backend host-cn      if host_cn
    use_backend host-xyz-url-xxx if host_xyz url_xxx
    default_backend default-servers

backend default-servers
    server default-servers1 127.0.0.1:8000 maxconn 32

backend host-abc-com
    balance roundrobin
    option httpcheck
    option forwardfor
    option httpclose
    option redispatch
    retries 3
    server abc-com1 192.168.1.100:80 check inter 2000 rise 2 fall 3 maxconn 32
    server abc-com2 192.168.1.101:80 check inter 2000 rise 2 fall 3 maxconn 32

backend host-cn
    balance roundrobin
    option httpcheck
    option forwardfor
    option httpclose
    option redispatch
    retries 3
    server abc-com1 192.168.1.100:80 check inter 2000 rise 2 fall 3 maxconn 32
    server abc-com2 192.168.1.101:80 check inter 2000 rise 2 fall 3 maxconn 32
    server abc-com3 192.168.1.102:80 check inter 2000 rise 2 fall 3 maxconn 32 backup

backend host-xyz-url-xxx
    balance roundrobin
    option httpcheck
    option forwardfor
    option httpclose
    option redispatch
    retries 3
    server abc-com1 192.168.1.100:80 check inter 2000 rise 2 fall 3 maxconn 32
    server abc-com2 192.168.1.101:80 check inter 2000 rise 2 fall 3 maxconn 32
```

包含listen区域的配置文件示例：

```
global
    daemon
    maxconn 256

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

listen http-in
    bind *:80
    server server1 127.0.0.1:8000 maxconn 32
```

本来想仔细讲解一下配置文件，但是发现HAProxy的配置太简单了，文档里写的非常清楚，我如果再详细说一遍的话就等于把HAProxy的文档翻译一遍，因此就不多说了。

工作的这一年多时间里，我感觉HAProxy是一个既简单又强大的代理，搭建起来非常容易，并且支持对HTTP协议头（请求头和响应头）的各种查询、替换和删除。前端时间同事给提了个需求，让HAProxy同时更改HTTP请求的Host和URI，需要将Host中的一个字段放在URI的最前面，由于HAProxy不支持变量的设定，因此无法完成这个需求（nginx支持set一个变量可以完成这个需求）。

如果你熟悉syslog(syslog-ng)的话，那么HAProxy日志的配置也很简单，这里不多说，请学习一下syslog相关知识，然后再来配置。

HAProxy做七层代理的话，一般需要和stunnel结合起来使用，stunnel把接收的HTTPS请求转发到HAProxy，然后再由HAProxy转发到后端服务器。

## stunnel相关配置介绍

stunnel配置比较简单，只有一个stunnel.conf配置文件，内容如下：

```
sslVersion=all
fips=no
cert=/etc/stunnel/abc.com.pem
CAfile=/etc/stunnel/abc.com.key
pid=/var/run/stunnel.pid
setuid=root
setgid=root
socket=l:TCP_NODELAY=1
socket=r:TCP_NODELAY=1
output=/var/log/stunnel.log

[https]
accept=443
connect=*:80
TIMEOUTclose=0
xforwardedfor=yes
```

就这样配置，stunnel就可以代理https请求的abc.com了。在stunnel4.44版本之前与HAProxy结合使用时都需要打一个HAProxy提供的[补丁][stunnel_patch]，但之后的版本就不需要打HAProxy的补丁了。

[stunnel_patch]: http://haproxy.1wt.eu/download/patches/

## haproxy使用

```
# haproxy -c -f /etc/haproxy/haproxy.cfg
# haproxy -D -f /etc/haporxy/haproxy.cfg
# haproxy -D -f /etc/haproxy/haproxy.cfg -p /var/run/haproxy.pid -sf $(cat /var/run/haproxy.pid)
```

注：很变态的是这些参数的位置不能随意更改。

## Haproxy 1.5

从1.5版开始，haproxy支持原生的配置ssl证书了，不需要再和stunnel配合使用了。并且TCP还支持sni，做代理翻墙很方便。


```
listen google-services
    bind *:443
    mode tcp
    option  tcplog
    use-server www.google.com if { req_ssl_sni -i www.google.com }
    use-server mail.google.com if { req_ssl_sni -i mail.google.com }
    server www.google.com www.google.com:443
    server mail.google.com mail.google.com:443
```

## Haproxy enable SSL

```
frontend https
    bind :443 ssl crt /etc/haproxy/server.pem
    default_backend mybackend

backend mybackend
    server s1 127.0.0.1:80
```

OR

```
server s2 jizhihuwai.com:443 crt /etc/haproxy/server.pem
```

`server.pem`是用`cat server.crt server.key > /etc/haproxy/server.pem`生成的。

`server.crt`一般是CA机构给颁发的，而`server.key`是你自己生成的private key。




参考博客：  
* [HAProxy负载均衡与keepalived搭建高可用负载均衡web（Nginx/PHP/Tomcat）集群](http://7424593.blog.51cto.com/7414593/1764640)
* [高负载均衡学习haproxy之安装与配置](https://www.ilanni.com/?p=9987)  
* [HAProxy文档（英文-1.7.9版）](https://cbonte.github.io/haproxy-dconv/1.7/configuration.html)  
* [生成自签名SSL证书](https://github.com/chenzhiwei/linux/tree/master/ssl-cert)



## [返回目录](https://github.com/MulticsYin/MulticsDevOps)
