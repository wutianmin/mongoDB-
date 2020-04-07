# MongoDB 

## 基本概念

+ 数据库（database）

数据库是一个仓库，在仓库中可以存放集合

+ 集合（collection）

集合类似于数组，在集合中可以存放文档

+ 文档（document）

文档是数据库中的最小单位，存储和操作的内容都是文档

### 三者关系

![correlation](C:\Users\wutianmin98\Desktop\TIM图片20200407110227.png)

一般就是对document进行操作

+ *集合和数据库都不需要手动创建*，当创建文档时，如果文档所在的集合或数据库不存在，会自动创建

## 基本指令

#### show dbs/show databases

显示当前所有数据库

#### use 数据库名称

进入指定数据库

#### db

db表示的是当前所处于的数据库

#### show collections

显示数据库中所有集合

## 数据库的CRUD操作

### 向数据库中插入文档

+ 向某集合中插入一个文档

```
db.集合名称.insert(doc)
```

例如：在stus集合中插入包括“姓名，年龄，性别”的一个文档 

```
db.stus.insert({name:"wtm",age:18,gender:"famale"});
```

+ 查询当前集合中的所有文档

```
db.集合名称.find()
```




















