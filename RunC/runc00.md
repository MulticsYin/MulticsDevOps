# RunC 源码解读

解读RunC源码，RunC GitHub地址：https://github.com/opencontainers/runc  

## 参考文章  
* [Docker背后的标准化容器执行引擎——runC](http://www.infoq.com/cn/articles/docker-standard-container-execution-engine-runc)
* [Docker背后的容器管理——Libcontainer深度解析](http://www.infoq.com/cn/articles/docker-container-management-libcontainer-depth-analysis?utm_source=infoq&utm_campaign=user_page&utm_medium=link)
* [Docker背后的内核知识——cgroups资源限制](http://www.infoq.com/cn/articles/docker-kernel-knowledge-cgroups-resource-isolation?utm_source=infoq&utm_campaign=user_page&utm_medium=link)
* [Docker背后的内核知识——Namespace资源隔离](http://www.infoq.com/cn/articles/docker-kernel-knowledge-namespace-resource-isolation?utm_source=infoq&utm_campaign=user_page&utm_medium=link)
* [DOCKER基础技术：LINUX NAMESPACE（上）](https://coolshell.cn/articles/17010.html)
* [DOCKER基础技术：LINUX NAMESPACE（下）](https://coolshell.cn/articles/17029.html)
* [DOCKER基础技术：LINUX CGROUP](https://coolshell.cn/articles/17049.html)
* [DOCKER基础技术：AUFS](https://coolshell.cn/articles/17061.html)
* [DOCKER基础技术：DEVICEMAPPER](https://coolshell.cn/articles/17200.html)
* [runC总体调用逻辑](http://blog.csdn.net/waltonwang/article/details/53892790)  
* [zhonglinzhang博客](http://blog.csdn.net/zhonglinzhang/article/category/3271199)

## [返回目录](https://github.com/MulticsYin/MulticsDevOps#dockerkubernetes)  

代码有点多，每天下班后花2~4小时阅读分析，随后再整理成为博客。

## [返回目录](https://github.com/MulticsYin/MulticsDevOps#dockerkubernetes)
