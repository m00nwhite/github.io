---
layout: post
title:  "ZooKeeper教程"
date:   2021-05-07 21:14:25 +0800
categories: zookeeper
---

# Zookeeper简介
ZooKeeper 是 Apache 软件基金会的一个软件项目，它为大型分布式计算提供开源的分布式配置服务、同步服务和命名注册。

ZooKeeper 的架构通过冗余服务实现高可用性。

简单来说zookeeper=文件系统的数据模型结构+监听通知机制（Watch）

Zookeeper的数据模型结构很像数据结构当中的树，也很像文件系统中的目录。

树是由所有节点组成,Zookeeper的数据存储也同样是基于节点，这种节点叫做Znode

但是，不同于树的节点，Znode的引用方式是路径引用，类似于文件系统，通过路径进行访问数据，而redis是根据key值来访问数据

## Zookeeper的安装
官网地址->  [Zookeeper](https://zookeeper.apache.org/releases.html)

![](https://yinyang.space/img/20210507_zookeeper02.png)

**安装、启动命令：**
```bash
tar xvf apache-zookeeper-3.7.0-bin.tar.gz
cd apache-zookeeper-3.7.0-bin
cd conf
cp zoo_sample.cfg zoo.cfg
cd ../bin
sh zkServer.sh start
```

**执行后，服务启动成功**
![](https://yinyang.space/img/20210507_zookeeper03.png)

**查看服务状态**
![](https://yinyang.space/img/20210507_zookeeper04.png)

## 配置文件
```properties
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just
# example sakes.
dataDir=/tmp/zookeeper
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
```
默认端口号是2181


## zookeeper 数据结构
zookkeeper 提供的名称空间非常类似于标准文件系统，整体结构类似于 linux 文件系统的模式以树形结构存储。其中根路径以 / 开头。所有存储的数据是由 znode 组成的，节点也称为 znode，并以 key/value 形式存储数据。
zookeeper 名称空间中的每个节点都是由一个路径标识.

![](https://yinyang.space/img/20210507_zookeeper01.png)


进入 zookeeper 安装的 bin 目录，通过sh zkCli.sh打开命令行终端，执行 "ls /" 命令显示：

```bash
./zkCli.sh
# 查看节点
[zk: localhost:2181(CONNECTED) 0] ls /
[zookeeper]

# 创建节点/yiny并赋值 123，创建节点时，必须要带上全路径
[zk: localhost:2181(CONNECTED) 1] create -e /yiny 123
Created /yiny

# 再次查看节点，确认除了zookeeper节点以外新增了yiny节点
[zk: localhost:2181(CONNECTED) 2] ls /
[yiny, zookeeper]

# 查看/zookeeper节点的内容
[zk: localhost:2181(CONNECTED) 4] ls /zookeeper
[config, quota]

# 使用get命令获取节点存储的数据
[zk: localhost:2181(CONNECTED) 5] get /yiny
123

# 使用set命令修改节点存储的数据
[zk: localhost:2181(CONNECTED) 6] set /yiny abc
[zk: localhost:2181(CONNECTED) 7] get /yiny
abc

# 使用delete命令删除节点
[zk: localhost:2181(CONNECTED) 8] delete /yiny
[zk: localhost:2181(CONNECTED) 9] ls /yiny
Node does not exist: /yiny
```

## ZooKeeper的基本操作
|命令|功能|
|---|---|
|create|创建节点|
|delete|删除节点|
|exists|判断节点是否存在|
|getData|获得一个节点的数据|
|setData|设置一个节点的数据|
|getChildren|获取节点下的所有节点|






