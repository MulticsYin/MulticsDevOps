# 给Docker配置官方国内加速镜像
出于网路原因，国内访问 Docker Hub 有时会遇到困难，下载官方镜像更是望穿秋水，可以配置镜像加速器，加速下载速度。  
Docker官方和国内很多云服务商都提供了加速器服务，常见有：
* [Docker 官方提供的中国registry mirror](https://docs.docker.com/registry/recipes/mirror/#use-case-the-china-registry-mirror)
* [阿里云加速器](https://cr.console.aliyun.com/#/accelerator)
* [DaoCloud 加速器](https://www.daocloud.io/mirror#accelerator-doc)  

在本文我们配置的是[Docker 中国官方镜像加速](https://www.docker-cn.com/registry-mirror)，镜像托管于国内，有更快的下载速度和更强的稳定性。该镜像库只包含流行的公有镜像。私有镜像仍需要从美国镜像库中拉取。  

### 一、直接使用命令直接从该镜像加速地址进行拉取
```bash
$ docker pull registry.docker-cn.com/NAME/REPO:TAG
```
例如:
```bash
$ docker pull registry.docker-cn.com/library/ubuntu:16.04
```
注:除非修改了 Docker 守护进程的 `--registry-mirror` 参数 (见下文), 否则仍需要完整地指定官方镜像的名称。  

### 二、使用 `--registry-mirror` 配置 Docker 守护进程
在 Docker 守护进程启动时传入 --registry-mirror 参数：  
```
$ docker --registry-mirror=https://registry.docker-cn.com daemon
```
### 二、配置系统文件(强烈推荐)
__1. 使用 `upstart` 系统 --> Ubuntu 14.04、Debian 7 Wheezy__  

编辑 /etc/default/docker 文件，在其中的 DOCKER_OPTS 中添加获得的加速器配置 --registry-mirror=<加速器地址>，如：
```
DOCKER_OPTS="--registry-mirror=https://registry.docker-cn.com"
```  
重新启动服务。  
```
$ sudo service docker restart
```  

__2. 使用 `systemd` 的系统 --> Ubuntu 16.04、Debian 8 Jessie、CentOS 7__  

用 systemctl enable docker 启用服务后，编辑 `/etc/systemd/system/multi-user.target.wants/docker.service` 文件，找到 ExecStart= 这一行，在这行最后添加加速器地址 --registry-mirror=<加速器地址>，如：
```
ExecStart=/usr/bin/dockerd --registry-mirror=https://registry.docker-cn.com
```
注：对于 1.12 以前的版本，dockerd 换成 docker daemon。  

重新加载配置并且重新启动。
```bash
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

## [返回目录](https://github.com/MulticsYin/MulticsDevOps)
