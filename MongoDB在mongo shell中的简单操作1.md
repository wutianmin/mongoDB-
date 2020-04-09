  > 关系型数据库（RDBMS)
  >
  > > + MySQL/Oracle/DB2/SQL Server
  > >+ 全是表格
  > > + 全部使用SQL语言

  > 非关系型数据库（No SQL）
  >
  > > + 键值对数据库
  > > + 文档数据库MongoDB 

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

#### `show dbs/show databases`

显示当前所有数据库

#### `use <database>`

进入指定数据库

#### `db`

db表示的是当前所处于的数据库

#### `show collections`

显示数据库中所有集合

## 数据库的CRUD操作

### 向数据库中插入文档 ==`insert()`==

+ 向集合中插入一个文档 ==`insertOne()`==

```
db.<collection>.insert({field:value})
```

例如：在stus集合中插入包括“姓名，年龄，性别”的一个文档 

```
db.stus.insert({name:"wtm",age:18,gender:"famale"});
```

+ 向集合中插入多个文档 ==`insertMany()`==
```
db.<collection>.insert([{field:value},{},{}...])
```

*当向集合中插入文档时，如果没有给文档指定_id属性，则数据库会自动为文档添加_id，该属性用来确保文档的唯一性*

*每调用一个`ObjectId`，会自动生成id，这是根据时间戳、机器码生成的，不会重复*

**文档中的value也可以是文档 ，如`insert([{field:{value1,value2,...}},{field:value},.......])`，这种情况下，称此文档为内嵌文档**

### 查询文档 

+ 查询集合中**所有**符合条件的文档 ==`find({})`==

```
db.<collection>.find()
```

`find({})`*{}*可以接收一个对象如，*{字段：值}* 作为条件参数

`find({})`返回的是一个**数组**，`find({})[1]`为返回所查询的文档中的第二个文档

+ 查询集合中符合条件的**第一个**文档 ==`findOne()`==

`findOne()`返回的是一个文档**对象**

+ 查询文档数量 ==`find().count()`==

**如果查询内嵌文档，field和value都必须加引号**
如
```
db.<collection>.find({'hobby.movies':'good'})
```

### 更新文档 ==`update()`==

```
db.<collection>.update(<query>,<update>)
db.<collection>.updateOne()或db.<collection>.replaceOne()
db.<collection>.updateMany()
```

*`update()`默认情况下会使用新对象来替换所有旧对象，且只改一个文档*

```
db.<collection>.update( 
<query>,
<update>,
{
upsert:<boolean>,
multi:<boolean>,
writeConcern:<document>,
collation:<document>
})
```

+ *如果需要修改多个文档，在命令中添加`multi:True`*

+ *如果需要修改指定的属性，而不是全部替换，需要使用**操作符***

  > [更多operators链接](https://docs.mongodb.com/manual/reference/operator/query/)

`$set`增加新的字段,`$unset`删除字段

```
{
<update operator>:{<field1>:<value1>,...}
}
```

### 更新数组

`$push`无论数组中是否有要添加的新的元素，都会添加进去

`$addToSet`不会更新数组中重复的元素

向数组中添加新的元素

### 删除文档 ==`delete()`、`remove()`==

```
db.<collection>.remove()
db.<collection>.deleteOne()
db.<collection>.deleteMany()
```

+ `remove()`可以根据条件来删除文档，传递条件的方式和find()一样，默认可以删除符合条件的所有文档，和`deleteMany()`作用相同

  **删除集合中的全部文档需要用`remove({})`，但此时并没有删除该集合，只是集合为空**

+ 如果要`remove()`只删除一个符合条件的文档，在直接在后面添加条件*true*，和`deleteOne()`作用相同

如

```
db.stus.remove({name:'wtm'},true)
```

### 删除集合、数据库

```
db.<collection>.drop()
db.dropDatabase()
```

*一般不会将数据库中的数据真正删除，实现是否删除的方法是在文档中插入一个`isDelete:0/1`的字段*

例如：

```
db.<collection>.insert([
 {
 name:'ty',isDelete:0
 },
 {
 name:'wtm',isDelete:0
 }
 ])
```

删除`name:'ty'`的文档

```
db.<collection>.update({name:'ty'},{$set:{isDelete:1}})
```

然后查询的时候增加条件`isDelete:0`即可
















