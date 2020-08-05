# Hive教程

## 概念

### 什么是Hive

Hive是基于Apache Hadoop的数据仓库基础设施，不是设计用来在线事务处理，适合传统数仓任务

### 数据单元

1. Database，命名空间，防止表名/视图/分块片/列等的冲突
2. Tables，相同schema的同质数据单元
3. Partitions，每张表可以有一个或多个分片键，用于决定数据如何存储，也于用户高效识别满足特定条件的行，如按日期或国家存储的分片
4. Buckets/Clusters，每个分片的数据可根据一些列的hash函数分为几个桶

### 类型系统

#### 原始类型

1. 整型
   1. TINYINT
   2. SMALLINT
   3. INT
   4. BIGINT
2. 布尔型
3. 浮点型
   1. FLOAT
   2. DOUBLE
4. 定点型
   1. DECIMAL
5. 字符串
   1. STRING
   2. VARCHAR
   3. CHAR
6. 日期与时间
   1. TIMESTAMP
   2. TIMESTAMP WITH LOCAL TIME ZONE
   3. DATE
7. 二值
   1. BINARY

#### 复杂类型

1. Structs
2. Maps
3. Arrays

## HiveServer2(HS2)

HiveServer2 (HS2) is a service that enables clients to execute queries against Hive，基于Thrift的Hive服务是HS2的核心，Thrift是一个跨平台RPC框架，包括四层，Server, Transport, Protocol, 和 Processor



