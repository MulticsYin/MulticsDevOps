# 如何免费的让网站启用HTTPS[[转](https://coolshell.cn/articles/18094.html)]

注：转载自[酷 壳 - CoolShell](https://coolshell.cn/)  

我用的是 [Let’s Encrypt](https://letsencrypt.org/)这个免费的解决方案。Let’s Encrypt 是一个于2015年推出的数字证书认证机构，将通过旨在消除当前手动创建和安装证书的复杂过程的自动化流程，为安全网站提供免费的SSL/TLS证书。这是由[互联网安全研究小组](https://letsencrypt.org/isrg/)（ISRG – Internet Security Research Group，一个公益组织）提供的服务。主要赞助商包括电子前哨基金会，Mozilla基金会，Akamai以及Cisco等公司（[赞助商列表](https://letsencrypt.org/sponsors/)）。  

2015年6月，Let’s Encrypt得到了一个存储在硬件安全模块中的离线的RSA根证书。这个由IdenTrust证书签发机构交叉签名的根证书被用于签署两个证书。其中一个就是用于签发请求的证书，另一个则是保存在本地的证书，这个证书用于在上一个证书出问题时作备份证书之用。因为IdenTrust的CA根证书目前已被预置于主流浏览器中，所以Let’s Encrypt签发的证书可以从项目开始就被识别并接受，甚至当用户的浏览器中没有信任ISRG的根证书时也可以。  
  
为网站来安装一个证书十分简单，只需要使用电子子前哨基金会EFF的 pCertbot](https://certbot.eff.org/)，就可以完成。  

* 首先，打开 https://certbot.eff.org 网页。
* 在机器上图标下面，选择你使用的 Web 接入软件 和你的 操作系统。比如：nginx 和 Ubuntu 16.04
* 然后就会跳转到一个安装教程网页。你就照着做一遍就好了。  

__以Nginx + Ubuntu16.04为例__
### 首先先安装相应的环境:
```bash
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx
```  
### 然后，运行如下命令:
```bash
$ sudo certbot --nginx
```  
`certbot` 会自动检查到你的 `nginx.conf` 下的配置，把你所有的虚拟站点都列出来，然后让你选择需要开启 https 的站点。你就简单的输入列表编号（用空格分开），然后，`certbot` 就帮你下载证书并更新 `nginx.conf` 了。  

你打开你的 nginx.conf 文件 ，你可以发现你的文件中的 server 配置中可能被做了如下的修改：  
```
listen 443 ssl; # managed by Certbot
ssl_certificate /etc/letsencrypt/live/coolshell.cn/fullchain.pem; # managed by Certbot
ssl_certificate_key /etc/letsencrypt/live/coolshell.cn/privkey.pem; # managed by Certbot
include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
```  
和  
```
# Redirect non-https traffic to https 
if ($scheme != "https") {
  return 301 https://$host$request_uri;
} # managed by Certbot 
```  
这里建议配置 http2，这要求 Nginx 版本要大于 1.9.5。HTTP2 具有更快的 HTTPS 传输性能，非常值得开启（关于性能你可以看一下这篇文章）。需要开启HTTP/2其实很简单，只需要在 nginx.conf 的 listen 443 ssl; 后面加上 http2 就好了。如下所示：  
```
listen 443 ssl http2; # managed by Certbot 
ssl_certificate /etc/letsencrypt/live/coolshell.cn/fullchain.pem; # managed by Certbot 
ssl_certificate_key /etc/letsencrypt/live/coolshell.cn/privkey.pem; # managed by Certbot 
include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
```  
然后，就 nginx -s reload 就好了。  

但是，Let’s Encrypt 的证书90天就过期了，所以，你还要设置上自动化的更新脚本，最容易的莫过于使用 crontab 了。使用 crontab -e 命令加入如下的定时作业（每个月都强制更新一下）：  
 ```
0 0 1 * * /usr/bin/certbot renew --force-renewal
5 0 1 * * /usr/sbin/service nginx restart
```
当然，你也可以每天凌晨1点检查一下：  
```
0 1 * * * certbot renew
```  

注：crontab 中有六个字段，其含义如下：  
第1个字段：分钟 (0-59)  
第2个字段：小时 (0-23)  
第3个字段：日期 (1-31)  
第4个字段：月份 (1-12 [12 代表 December])  
第5个字段：一周当中的某天 (0-7 [7 或 0 代表星期天])  
```
/path/to/command – 计划执行的脚本或命令的名称
```  

这么方便的同时，我不禁要问，如果是一些恶意的钓鱼网站也让自己的站点变成https的，这个对于一般用来说就有点难以防范了。哎……  

当然，在nginx或apache上启用HTTPS后，还没有结束。因为你可能还需要修改一下你的网站，不然你的网站在浏览时会出现各种问题。  

启用HTTPS后，你的网页中的所有的使用 http:// 的方式的地方都要改成 https:// 不然你的图片，js， css等非https的连接都会导致浏览器抱怨不安全而被block掉。所以，你还需要修改你的网页中那些 hard code http:// 的地方。  

对于我这个使用wordpress的博客系统来说，有这么几个部分需要做修改。  
* 首先是 wordpress的 常规设置中的 “WordPress 地址” 和 “站点地址” 需要变更为 https 的方式。
* 然后是文章内的图片等资源的链接需要变更为 https 的方式。对此，你可以使用一个叫 “Search Regex” 插件来批量更新你历史文章里的图片或别的资源的链接。比如：把 http://coolshell.cn 替换成了 https://coolshell.cn
* 如果你像我一样启用了文章缓存（我用的是WP-SuperCache插件），你还要去设置一下 “CDN” 页面中的 “Site URL” 和 “off-site URL” 确保生成出来的静态网页内是用https做资源链接的。  

基本如此。希望大家都来把自己的网站更新成 https 的。  
  
   [返回目录](https://github.com/MulticsYin/MulticsDevOps)
