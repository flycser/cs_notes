# HBase教程

## 部署方式

HBase守护进程，Master, RegionServers, ZooKeeper

单机版(standalone mode)，所有守护进程都运行在一个jvm进程里，运行在一台主机上，数据存储在本地文件系统，单机版安装过程如下，适用于hbase 2.1.x

1. 下载tar包，解压文件

2. 修改配置文件，添加配置项

   ```xml
   <!-- HBase存储数据的路径 默认在tmp路径下，不安全 -->
   <configuration>
   	<property>
   		<name>hbase.rootdir</name>
   		<value>PATH</value>
   	</property>
   </configuration>
   ```

3. 进入主目录，$HBASE_HOME，启动脚本"bin/hbase-start.sh"
4. 进入shell，"./hbase shell"

伪分布式(pseudo-distributed)，每个守护进程(Master, RegionServer, ZooKeeper)作为一个单独进程运行，仍然运行在一台主机上，数据可以存储在HDFS或本地文件系统

完全分布式(fully distributed)，集群包含多个节点，每个节点运行一个或多个HBase守护进程，包括主要与备份Master实例，多个ZooKeeper节点，和多个RegionServer节点

### Java API连接配置

HBase的RegionServer会将自己的hostname上报到zookeeper，客户端连接zookeeper时，获取的是regionserver的hostname，再由hostname获得regionserver的ip地址。基于hbase的这种名称上报机制，客户端连接hbase时，需要能够ping通hbase的hostname，但是如果把hbase的hostname分发到所有的服务器上，毕竟是不现实的。一种可选择的方法，就是以hbase的hostname作为域名配置到DNS，这样客户端服务器也能够通过ping hostname获取服务器的实际ip

**hbase默认通过hostname寻找ip**，然后将ip注册到zookeeper中作为hbase单机服务的ip地址，所以我们需要修改/etc/hosts文件，本地hostname为localhost.localdomain，原本指向127.0.0.1，修改为hbase部署及其ip地址，并设置本地域名为hbaseserver，注意，本地和远程部署hbase服务器都要改hosts文件

```xml
xxx.xxx.xxx.xxx(IP) hbaseserver(dns)
```

conf/hbase-site.xml配置

```xml
<configuration>
	<property>
		<name>hbase.zookeeper.property.clientPort</name>
		<value>2181</value>
	</property>
	<property>
  	<name>hbase.master.info.port</name>
		<value>16010</value>
	</property>    
	<!--客户端基于zookeeper连接hbase的zookeeper地址  其他的都是默认的端口号-->
	<property>
		<name>hbase.zookeeper.quorum</name>
		<value>hbaseserver</value>
	</property>
	<property>
		<name>hbase.regionserver.port</name>
		<value>16020</value>
	</property>
	<property>
		<name>hbase.regionserver.info.port</name>
		<value>16030</value>
	</property>  
</configuration>
```

maven pom.xml

```xml
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>2.1.x</version>
</dependency>
```

连接代码可参考官网示例

## Apache HBase配置

### 配置文件

Apache HBase使用与Apache Hadoop相同的配置系统，所有的配置文件位于conf/目录

#### HBase配置文件描述

##### backup-masters

普通文本文件，列出那些Master应该启动一个备份Master进程，一行一个主机

##### hadoop-metrics2-hbase.properties

用于连接HBase Hadoop‘s Metrics2框架

##### hbase-env.cmd and hbase-env.sh

在Windows和Linux/Unix系统上用于设置HBase工作环境的脚本，包括Java，Java选项，和其他环境变量

##### hbase-policy.xml

RPC服务器在客户请求时做授权决策的默认policy文件，只有在HBase security开启时使用

##### hbase-site.xml

HBase主配置文件，这个文件的配置选项会覆盖HBase的默认配置，默认配置文件时docs/hbase-default.xml，也可以在HBase Web UI的HBase Configuration tab查看集群的生效配置

##### log4j.properties

HBase通过log4j打印日志的配置文件

##### regionservers

普通文本文件，包含一个在集群中应该运行RegionServer主机列表，默认这个文件只包含一个localhost，该文件应该包含主机名或IP地址，每行一个，如果集群只在localhost运行一个RegionServer，则只包含localhost

### 基本先决条件

##### Java

##### 操作系统工具

1. ssh
2. DNS 
3. NTP
4. 文件和进程数限制(ulimit)
   1. 许多Linux版本对单个用户可打开文件数有限制

5. Linux Shell

#### Hadoop

版本匹配，参见官网

#### ZooKeeper要求

ZooKeeper 3.4.x

### HBase运行模式: 单机与分布式

#### 单机模式HBase

数据持久化在本地文件系统或HDFS上

#### 伪分布式

所有守护进程运行在一个节点上，数据持久化在本地文件系统或HDFS上

#### 完全分布式

守护进程分布在集群上的不同节点上，只可以持久化在HDFS上

### 运行与确认安装

1. HDFS(如果需要的话)

2. ZooKeeper(默认HBase管理或自己管理)

3. 运行HBase，在HBase主目录

   ```shell
   ./bin/start-hbase.sh
   ```

4. Web UI，运行于Master所在主机，端口16010，RegionServers监听16020，HTTP server监听16030

5. 关闭HBase

   ```shell
   ./bin/stop-hbase.sh
   ```

### 默认配置

#### hbase-site.xml and hbase-default.xml

#### HBase默认配置

### 示例配置

### 重要配置

### 动态配置

## The Apache HBase Shell

## Data Model

在HBase中，数据存储在表格中，表格拥有行列

#### HBase数据模型术语

##### Table

一个HBase表包括多行

##### Row

一行包括一个行键(row key)，以及与键关联的一个或多个列，行存储按row key字母序排序

##### Column

一列包括一个列族(column family)和一个列修饰符(column qualifier)，由冒号:分隔

##### Column Family

物理上与column和它们的值同位置，用于性能提升，每个column family拥有一系列存储属性，如它的值是否在内存缓存，数据如何压缩或者row keys如何编码等等，一个表中的一行拥有相同的column families，尽管一行不一定存储column family中的所有？

##### Column Qualifier

列修饰符被添加在一个column family上，给一段数据提供索引，column families在表创建时即固定，但column qualifiers可变而且汗之间差别巨大

##### Cell

一个cell是row，column family和column qualifier的组合，包含值和时间戳(timestamp)

cell -> {(table, row key, column family, column qualifier, timestamp) -> value}

##### Timestamp

时间戳伴随每个数据，是数据版本的标识符，磨人地，时间戳表示数据写入RegionServer的时间，但是在存放数据到cell时可以指定一个不同的时间戳

### Conceptual View

示例 webtable

| Row Key           | Time Stamp | **ColumnFamily** contents | **ColumnFamily** anchor       | **ColumnFamily** people    |
| ----------------- | ---------- | ------------------------- | ----------------------------- | -------------------------- |
| "com.cnn.www"     | t9         |                           | anchor:cnnsi.com = "CNN"      |                            |
| "com.cnn.www"     | t8         |                           | anchor:my.look.ca = "CNN.com" |                            |
| "com.cnn.www"     | t6         | contents:html = "<html>…" |                               |                            |
| "com.cnn.www"     | t5         | contents:html = "<html>…" |                               |                            |
| "com.cnn.www"     | t3         | contents:html = "<html>…" |                               |                            |
| "com.example.www" | t5         | contents:html = "<html>…" |                               | people:author = "John Doe" |

### Physical View

*ColumnFamily* `anchor`

| Row Key       | Time Stamp | Column Family `anchor`          |
| :------------ | :--------- | :------------------------------ |
| "com.cnn.www" | t9         | `anchor:cnnsi.com = "CNN"`      |
| "com.cnn.www" | t8         | `anchor:my.look.ca = "CNN.com"` |

*ColumnFamily* `contents`

| Row Key       | Time Stamp | ColumnFamily `contents:`  |
| :------------ | :--------- | :------------------------ |
| "com.cnn.www" | t6         | contents:html = "<html>…" |
| "com.cnn.www" | t5         | contents:html = "<html>…" |
| "com.cnn.www" | t3         | contents:html = "<html>…" |

## Namespace

一个namespace是一个逻辑上的表集合，类似关系型数据库中的数据库概念

## HBase and Schema Design

## Apache HBase APIs

