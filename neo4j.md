# Neo4j教程

## 图数据库概要

### 相关概念

一个图由无数的节点和关系组成

“节点 — 被组织 → 关系 — 可以有 → 属性” 关系可以将节点组织成任意的结构，允许一张图被组织成一个列表，一棵树，一张地图，或者一个复杂的实体 – 这个实体本身也是由复杂的，关系高度关联的结构组成

“一个 Traversal — 导航 → 一张图; 他 — 标示 → 路径 — 包含 → 节点” 一次 Traversal, 你可以理解为是你通过一种算法，从一些开始节点开始查询与其关联的节点

“一个索引 — 映射到 → 属性 — 属于 → 节点或者关系” 经常，你想通过某一给定的属性值找到节点或者关系，比起通过遍历我们的图来书，用索引将会更加高效

以下为Neo4j数据库中的一些概念

#### Nodes

节点用来表示实体

#### Labels

标签用来设定领域，拥有相同标签的节点属于同一个集合

#### Relationships

一个关系连接两个节点，一个关系总有方向

1. 关系在任一方向都会被遍历访问，这意味着我们并不需要在不同方向都新增关系

2. 而关系总是会有一个方向，所以当这个方向对你的应用没有意义时你可以忽略方向

3. 特别注意一个节点可以有一个关系是指向自己

#### Relationship types

一个关系有且仅有一种关系类型

#### Properties

属性是名-值对(name-value pairs)，用于对节点或关系添加特质，节点和关系都可以设置自己的属性，属性是由Key-Value键值对组成，键名是字符串，属性值是要么是原始值，如string，int等，要么是原始值类型的一个数组

#### Traversals and paths

1. 遍历(traversal)是指如何查询图以便寻找问题的答案，遍历一个图意味着根据一些规则按一些关系访问关联的节点集合
2. 路径由至少一个节点，通过各种关系连接组成，经常是作为一个查询或者遍历的结果。

#### Schema

Neo4j中的schema是指索引(indexes)与约束(constraints)，在Neo4j中，schema是可选的，也就是没有必要一定创建索引与约束，不必预先定义schema，为了改善性能和方便建模，可以考虑引入schema，索引用于改善性能，约束保证数据符合领域规则

### 从图数据库转换成RDBMS

### 从图数据库转换成Key-Value数据库

Key-Value模型适合用于简单的数据或者列表。当数据之间不断交互关联时，更需要一张图模型。

### 从图数据库转换成列数据库

列式（大表）数据库是 Key-Value模型的升级，用 “”来允许行数据增加。如果存储一张图，这个表将是分层的，关系也是非常明确的？？？

### 从图数据库转换成文档型数据库

文档型数据库用文档进行层次划分，而自由的数据规划也很容易被表示成一颗树。成长为一张图的话，文档之间的关联你需要更有代表性的数据结构来存储

## Cypher手册

CQL表示Cypher Query Language

### CQL简介

常用的命令与子句如下

| 命令/子句 | 用途                               |
| --------- | ---------------------------------- |
| CREATE    | 创建节点，关系和属性               |
| MATCH     | 检索满足条件的节点，关系和属性数据 |
| RETURN    | 返回查询结果                       |
| WHERE     | 提供条件筛选检索数据               |
| DELETE    | 删除节点和关系                     |
| REMOVE    | 删除节点和关系的属性               |
| ORDER BY  | 对检索的数据进行排序               |
| SET       | 要添加或更新标签                   |

常用的函数如下

| 函数   | 用法                                                         |
| ------ | ------------------------------------------------------------ |
| 字符串 | 用来处理字符串，如UPPER，LOWER，SUBSTRING，                  |
| 聚合   | 用来对CQL查询结果执行一些聚合操作，如COUNT，MAX，MIN，SUM，AVG |
| 关系   | 用来得到关系的起始节点，终止节点等细节信息，如STARTNODE，ENDNODE，ID，TYPE |

支持的数据类型如下

| 类型    | 描述                                                    | 取值范围                    |
| ------- | ------------------------------------------------------- | --------------------------- |
| boolean | true/false                                              |                             |
| byte    | 8-bit integer                                           | -128 to 127, inclusive      |
| short   | 16-big integer                                          | -32768 to 32767, inclusive  |
| int     | 32-bit integer                                          |                             |
| long    | 64-bit integer                                          |                             |
| float   | 32-bit IEEE 754 floating-point number                   |                             |
| double  | 64-bit IEEE 754 floating-point number                   |                             |
| char    | 6-bit unsigned integers representing Unicode characters | u0000 to uffff (0 to 65535) |
| String  | sequence of Unicode characters                          |                             |

与Java数据类型相似

### 创建节点

#### 创建无属性节点

```cypher
CREATE (<node-name>:<label-name>)
```

#### 创建带属性节点

```cypher
CREATE (
   <node-name>:<label-name>
   { 	
      <Property1-name>:<Property1-Value>,
      ........
      <Propertyn-name>:<Propertyn-Value>
   }
)
```

### MATCH命令

```cypher
MATCH (
   <node-name>:<label-name>
)
```

### RETURN子句

```cypher
RETURN 
   <node-name>.<property1-name>,
   ........
   <node-name>.<propertyn-name>
```

### MATCH及RETURN

MATCH或RETURN命令不能单独使用，要结合这两个命令来检索数据库中的数据

```cypher
MATCH Command
RETURN Command
```

### 关系基础

按图模型，关系应该是方向性的，否则，Neo4j的将抛出一个错误消息，基于方向性，Neo4j的关系被分为两种主要类型，单向关系和双向关系，所有关系有出(outcoming)与入(incoming)

### 创建标签

#### 创建节点的单个标签

```cypher
CREATE (<node-name>:<label-name>)
```

#### 创建节点的多个标签


```cypher
CREATE (<node-name>:<label-name1>:<label-name2>.....:<label-namen>)
```

#### 创建节点与关系的标签

```cypher
CREATE (<node1-name>:<label1-name>)-
		[(<relationship-name>:<relationship-label-name>)]
		->(<node2-name>:<label2-name>)
```

### WHERE子句

WHERE子句在CQL中用来筛选匹配查询结果

#### 简单的WHERE子句语法 

```cypher
WHERE <condition>
```

#### 复杂的WHERE子句语法 

```cypher
WHERE <condition> <boolean-operator> <condition>
```

#### \<condition>语法

```cypher
<property-name> <comparison-operator> <value>
```

#### 创建具有WHERE子句的两个已存在节点之间的关系

```cypher
MATCH (<node1-label-name>:<node1-name>),(<node2-label-name>:<node2-name>) 
WHERE <condition>
CREATE (<node1-label-name>)-[<relationship-label-name>:<relationship-name>
       {<relationship-properties>}]->(<node2-label-name>) 
```

### 删除

#### 删除节点

```cypher
DELETE <node-name-list>
```

**注意**，应该使用逗号","运算符分隔节点名称

#### 删除节点和关系子句语法 

```cypher
DELETE <node1-name>,<node2-name>,<relationship-name>
```

## 移除

#### 移除节点/关系的属性 

可以使用REMOVE子句来移除节点或关系的属性

```cypher
REMOVE <property-name-list>
```

#### \<property-name-list>语法 

```cypher
<node-name>.<property1-name>,
<node-name>.<property2-name>, 
.... 
<node-name>.<propertyn-name> 
```

#### 删除节点/关系的标签

可以使用相同语法删除节点或关系的标签

```cypher
REMOVE <label-name-list>
```

#### \<label-name-list>语法 

```cypher
<node-name>:<label1-name>,
<node-name>:<label2-name>, 
.... 
<node-name>:<labeln-name> 
```

### SET子句

### ORDER BY子句

### UNION子句

### LIMIT和SKIP子句

### MERGE命令

### NULL

### IN操作符

### 字符串函数

### 聚合函数

### 关系函数

#### 获取所有节点Labels

```cypher
MATCH (a) RETURN DISTINCT LABELS(a)
```

#### 获取所有节点的属性名

```cypher
MATCH (a) RETURN DISTINCT KEYS(a)
```



## Driver手册 (Java)

```xml
<dependency>
  <groupId>org.neo4j.driver</groupId>
  <artifactId>neo4j-java-driver</artifactId>
  <version>1.7.5</version>
</dependency>
```

### 连接数据库

```java
import org.neo4j.driver.v1.*;

Driver driver = GraphDatabase.driver(uri, AuthTokens.basic(username, password));
```

### 查询

```java

```



### 插入

```java

```







## **Neo4j ETL工具**

Neo4j ETL (Extract-Transform-Load)工具的构建使开发人员能够轻松地将关系数据加载到图数据库中，它包括3个简单的步骤来实现：

1. 通过JDBC设置指定源关系数据库
2. 使用图形化的编辑工具建立数据模型映射
3. 运行生成的脚本将所有数据导入到Neo4j

Neo4j ETL App会根据源数据库模式决定基本的数据映射，规则如下：

1. 拥有1个外键的表会映射成节点和其上的关系
2. 拥有2个外键的表会被当做是关系表，映射成关系
3. 拥有多于2个外键的表会被当作中间表处理，映射成拥有多个关系的节点



## 参考文献

1. [Neo4j官方英文文档](https://neo4j.com/docs/)
2. [Neo4j中文文档](http://neo4j.com.cn/public/docs/index.html)