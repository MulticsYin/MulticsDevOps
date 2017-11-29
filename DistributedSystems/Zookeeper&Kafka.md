# Zookeeper&Kafka集群配置

前期准备：  
1. 三台Linux主机(本文采用ubuntu16.04 Server)。
2. JAVA JDK，Zookeeper，Kafka安装包。

## 一、构建基础环境
```
1. 在虚拟机中安装三个Linux系统。（本机虚拟机使用VMware，Linux使用Ubuntu16.04 server版本）
2. 配置文件：
    1) 分别修改三台主机“/etc/hostname”文件，分别改为cluster00, cluster01, cluster02。
    2) 分别修改三台主机“/etc/hosts”文件，如附件“hosts”一致，注意自己配置时IP可能不一致，使用“ifconfig”命令查询出来替换即可。
3. 新建配置目录并设置权限：
    1) root 用户下执行“mkdir /cluster”命令，在根目录下新建“/cluster”目录。
    2) 修改用户所有者和用户组所有者，命令：chown multics /cluster; chgrp multics /cluster
4. 下载JDK，Zookeeper，Kafka安装包到当前主机，使用“scp”命令将文件复制到三台虚拟机“/cluster/software”文件夹里面。
5. 新建“/cluster/data/zookeeper”和“/cluster/data/kafka”目录
```  
## 二、配置Zookeeper集群环境
Kafka集群运行依赖Zookeeper集群，所以我们先配置Zookeeper集群。  
```
1. 新建“/cluster/server/zookeeper”目录。
2. 解压 Zookeeper 压缩包到“/cluster/server/zookeeper”目录。
3. 拷贝附录文件“zoo.cfg”到“/cluster/server/zookeeper/conf”目录。
4. 在“dataDir=/cluster/data/zookeeper”配置属性所配置的文件夹中新建“myid”文件，写入“1”
5. 依照前四步配置其他两台服务器，“myid”文件分别写入“2”和“3”。
5. 启动zookeeper，分别在三台主机运行：/cluster/server/zookeeper/bin/zkServer.sh start
6. 查看运行状态：/cluster/server/zookeeper/bin/zkServer.sh status 如下显示则正常。
    ZooKeeper JMX enabled by default
    Using config: /cluster/server/zookeeper/bin/../conf/zoo.cfg
    Mode: leader	# 该行或者显示Mode: follower
7. 停止zookeeper集群：/cluster/server/zookeeper/bin/zkServer.sh stop
```  
## 三、配置Kafka集群
```
1. 新建“/cluster/server/kafka”目录。
2. 解压 Zookeeper 压缩包到“/cluster/server/kafka”目录。
3. 拷贝附录文件“server.properties”到“/cluster/server/kafka/config”目录，替换原有的文件。
4. 按以上三个步骤分别部署其他两台主机。
5. cluster00, cluster01和cluster02主机上分别修改对应参数：
    1) broker.id=1  # 对应主机1,2,3
    2) host.name=192.168.0.183 # 对应主机IP
    3) zookeeper.connect=192.168.0.183:2181,192.168.0.135:2181,192.168.0.144:1218 # 修改成为自己集群IP
6. 启动Kafka集群，三台主机分别运行：/cluster/server/kafka/bin/kafka-server-start.sh -daemon /cluster/server/kafka/config/server.properties
7. 停止kafka集群，三台主机分别运行：/cluster/server/kafka/bin/kafka-server-stop.sh
```
## 四、常用命令

__cluster00__
```
1. start cluster00
    1) /cluster/server/zookeeper/bin/zkServer.sh start
    2) /cluster/server/zookeeper/bin/zkServer.sh status
    3) /cluster/server/kafka/bin/kafka-server-start.sh -daemon /cluster/server/kafka/config/server.properties

2. producer
    1) /cluster/server/kafka/bin/kafka-console-producer.sh --broker-list 192.168.1.22:9092 --topic multics

3. stop cluster00
    1) /cluster/server/kafka/bin/kafka-server-stop.sh
    2) /cluster/server/zookeeper/bin/zkServer.sh stop
```

__cluster01__
```
1. start cluster01
    1) /cluster/server/zookeeper/bin/zkServer.sh start
    2) /cluster/server/zookeeper/bin/zkServer.sh status
    3) /cluster/server/kafka/bin/kafka-server-start.sh -daemon /cluster/server/kafka/config/server.properties

2. consumer
    1) /cluster/server/kafka/bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.22:9092 --topic multics --from-beginning

3. stop cluster01
    1) /cluster/server/kafka/bin/kafka-server-stop.sh
    2) /cluster/server/zookeeper/bin/zkServer.sh stop
```

__cluster02__
```
1. start cluster02
    1) /cluster/server/zookeeper/bin/zkServer.sh start
    2) /cluster/server/zookeeper/bin/zkServer.sh status
    3) /cluster/server/kafka/bin/kafka-server-start.sh -daemon /cluster/server/kafka/config/server.properties

2. consumer
    1) /cluster/server/kafka/bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.22:9092 --topic multics --from-beginning

3. stop cluster02
    1) /cluster/server/kafka/bin/kafka-server-stop.sh
    2) /cluster/server/zookeeper/bin/zkServer.sh stop
```

## [返回目录](https://github.com/MulticsYin/MulticsDevOps#分布式系统相关组件)
