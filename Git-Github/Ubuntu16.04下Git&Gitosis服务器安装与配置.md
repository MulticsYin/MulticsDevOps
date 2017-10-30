Ubuntu16.04下Git&Gitosis服务器安装与配置
===
__本教程为在实验室或者公司内部搭建一个局域网的git服务器。__  

资源需求：
* 有一台主机，安装Linux系统（我这里是Ubuntu16.04），安装git，openssh等工具（见下文）。  

功能需求：  
* 搭建一个局域网的本地git服务器，用于保存代码。
* 有相应的权限，每个组有每个组的权限（类似于Linux用户权限之类）。

#### 1. 安装
##### 1.1 安装Git:
```bash
$ sudo apt-get install git git-core
```
##### 1.2 安装 Gitosis
```bash
$ sudo apt-get install python-setuptools
$ mkdir ~/git; cd ~/git; git clone https://github.com/res0nat0r/gitosis.git
$ cd gitosis; sudo python setup.py install
```
##### 1.3 安装 openssh服务器
```bash
$ sudo apt-get install openssh-server openssh-client
```
##### 1.4 增加名为Git的用户
```bash
$ sudo adduser --system --shell /bin/bash  --gecos 'git version control' --group --disabled-password --home /home/git git
$ sudo passwd git
```
##### 1.5 上传公钥(如果没有，用 ssh-keygen -t rsa 生成)到Git服务器
本地客户端操作
```bash
$ scp .ssh/id_rsa.pub git@YOUR_SERVER:/home/git（YOUR_SERVER换成你服务器IP或域名）
```
Git服务器操作
```bash
$ sudo -H -u git gitosis-init < /home/git/id_rsa.pub
$ sudo chmod 755 /home/git/repositories/gitosis-admin.git/hooks/post-update
```
#### 2. 配置
##### 2.1 修改配置文件
```bash
$ git clone git@YOUR_SERVER:gitosis-admin.git
```
成功后，在本地将有一个gitosis-admin目录，里面有gitosis.conf，keydir。  
##### 2.2 编辑gitosis.conf，添加如下内容
```bash
[group GROUP_NAME] 
writable = GROUP_PROJECY 
members = GROUP_MEMBERS
```
注：GROUP_NAME是组名；GROUP_PROJECY是项目名称；GROUP_MEMBERS是项目成员，可多个。
##### 2.3 提交修改
```bash
$ git add ./*
$ git commit -a -m "created a new repository" 
$ git push
```
##### 2.4 新建Git项目 - 服务端操作
```bash
$ cd /home/git/repositories/
$ git init --bare GROUP_PROJECY
```
##### 2.5 添加Git成员
###### 2.5.1 scp获取成员公钥，拷贝到keydir文件夹: 
```bash
$ cp ~/.ssh/id_rsa.pub ~/.ssh/GROUP_MEMBERS.pub (GROUP_MEMBERS为项目成员，注意要与“keydir”目录下私钥名称一致，参见默认设置)
$ scp ~/.ssh/id_rsa.pub git@YOUR_SERVER:/home/git（YOUR_SERVER换成你服务器IP或域名）
$ cd gitosis-admin 
$ cp ~/.ssh/GROUP_MEMBERS.pub keydir/ 
$ git add keydir/GROUP_MEMBERS.pub
```
###### 2.5.2 修改gitosis.conf
```
[group GROUP_NAME] 
writable = GROUP_PROJECY 
members = GROUP_MEMBERS <-- 此处添加成员名称
```
###### 2.5.3 提交修改
```bash
$ git add ./*
$ git commit -a -m "Add user" 
$ git push
```
###### 2.5.4 这样，其它成员就可以获取代码了
```bash
$ git clone git@YOUR_SERVER:GROUP_PROJECY.git
```
#### 3. 远程拷贝文件:
##### 3.1. 从本地服务器复制到远程服务器
* 复制文件
```bash
$ scp local_file remote_username@remote_ip:remote_folder
$ scp local_file remote_username@remote_ip:remote_file
$ scp local_file remote_ip:remote_folder
$ scp local_file remote_ip:remote_file
```
指定了用户名，命令执行后需要输入用户密码；如果不指定用户名，命令执行后需要输入用户名和密码；  
* 复制目录
```bash
$ scp -r local_folder remote_username@remote_ip:remote_folder
$ scp -r local_folder remote_ip:remote_folder
```
第1个指定了用户名，命令执行后需要输入用户密码； 第2个没有指定用户名，命令执行后需要输入用户名和密码；  

注: 从远程复制到本地的scp命令与上面的命令一样，只要将从本地复制到远程的命令后面2个参数互换顺序就行了。  
  
  [返回目录](https://github.com/MulticsYin/MulticsDevOps)
