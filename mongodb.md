# MongoDB教程

## MongoDB简介

MongoDB是**文档数据库**(Document Database)，MongoDB中的一条记录指一篇文档，文档是一种由键值对组成的数据结构。MongoDB文档类似JSON对象，键值对的值可以是其他文档，数组，或文档数组。

适用文档的优势是

1. 文档对应许多编程语言的原生数据类型
2. 嵌入文档与数组减少了成本高昂的join操作
3. 动态模式支持流畅的多态性(fluent polymorphism)

MongoDB将文档存储在collections中，类似关系型数据库中表的概念。MongoDB也支持只读视图与请求式实体化视图。

### Databases与Collections

MongoDB存储BSON文档，即数据记录在数据库的collections中。

Databases存储文档的collections。

#### Views

A MongoDB view is a queryable object whose contents are defined by an [aggregation pipeline](https://docs.mongodb.com/manual/core/aggregation-pipeline/#id1) on other collections or views. MongoDB does not persist the view contents to disk. 

Views只读，使用原始collection的索引，无法构建，删除，或重建views之上的索引，无法指定view的自然排序，不可重命名，不支持一些projection操作符

#### On-Demand Materialized Views

### Documents

### BSON Types