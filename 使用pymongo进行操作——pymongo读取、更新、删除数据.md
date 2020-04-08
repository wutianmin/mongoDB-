## 使用运算符和复合查询读取数据

使用==`find()`==命令

### 使用嵌入式字段和比较运算符读取数据

> 查询运算符 `$`
>
> > [`$`使用方法链接](https://docs.mongodb.com/manual/reference/operator/query/)

*# 在这里将一些比较常用的比较符号归纳如下表：*

| 表达式 |    含义    |            示例             |
| :----: | :--------: | :-------------------------: |
| `$lt`  |    小于    |    {'age': {'$lt': 20}}     |
| `$gt`  |    大于    |    {'age': {'$gt': 20}}     |
| `$lte` |  小于等于  |    {'age': {'$lte': 20}}    |
| `$gte` |  大于等于  |    {'age': {'$gte': 20}}    |
| `$ne`  |   不等于   |    {'age': {'$ne': 20}}     |
| `$in`  |  在范围内  | {'age': {'$in': [20, 23]}}  |
| `$nin` | 不在范围内 | {'age': {'$nin': [20, 23]}} |

*# 另外还可以进行正则匹配查询，例如查询名字以M开头的学生数据，示例如下：*

`results = collection.find({'name': {'$regex': '^M.*'}})`

*在这里使用了`$regex`来指定正则匹配，`^M`代表以M开头的正则表达式，这样就可以查询所有符合该正则的结果。*

*#在这里将一些功能符号再归类如下：*

|  表达式   |     含义     |                             示例                             |
| :-------: | :----------: | :----------------------------------------------------------: |
| `$regex`  |   匹配正则   |           {'name': {'$regex': '^M.*'}}name以M开头            |
| `$exists` | 属性是否存在 |           {'name': {'$exists': True}}name属性存在            |
|  `$type`  |   类型判断   |           {'age': {'$type': 'int'}}age的类型为int            |
|  `$mod`   |  数字模操作  |             {'age': {'$mod': [5, 0]}}年龄模5余0              |
|  `$text`  |   文本查询   | {'$text': {'$search': 'Mike'}}text类型的属性中包含Mike字符串 |
| `$where`  | 高级条件查询 | {'$where': 'obj.fans_count == obj.follows_count'}自身粉丝数等于关注数 |

1 连接test数据库

```2
db = client.test
```

2 使用小于运算符`$lt`选择文档

例如，在inventory集合中，从`size`字段的`h`中选择`h`小于15的所有文档，然后遍历结果

```python
cursor = db.inventory.find({"size.h": {"$lt": 15}})
from pprint import pprint
for inventory in cursor:
	pprint(inventory)
```

### 使用复合查询读取数据

使用*AND*和*OR*逻辑符读取数据，形成复合查询

+ 隐式*AND*查询：用逗号分隔开每个表达式

在查找文档中指定要匹配的所有字段，然后遍历结果

例如，在inventory集合中，读取所有*`status="A"`*和*`qty<30`*的文档

```python
cursor = db.inventory.find({"status": "A", "qty": {"$lt": 30}})
```

* 直接使用*AND*逻辑符

`{ $and: [ { <expression1> }, { <expression2> } , ... , { <expressionN> } ] }`

```python
cursor=db.inventory.find({"$and":[{"status":"A"},{"qty":{"$lt":30}}]})
```

+ *OR*逻辑符

例如，在inventory集合中，读取所有*`status="A"`*或*`qty<30`*的文档

```python
cursor = db.inventory.find(
    {"$or": [{"status": "A"}, {"qty": {"$lt": 30}}]})
```

+ 使用多个逻辑符进行复合查询

例如，在inventory集合中，读取所有*`status="A"`*和（*`qty<30`*或item中以字母p开头）的文档

```python
cursor = db.inventory.find({
    "status": "A",
    "$or": [{"qty": {"$lt": 30}}, {"item": {"$regex": "^p"}}]})
```



## 更新数据

### 在inventory集合中更新一个文档

==使用`update_one()`命令==

mongoDB提供更新操作符来改变字段的值

在一些更新操作符中，如果文档中不存在某个更新的值所在的字段，那么这个字段会被创建

+ `$set`用来更新字段

+ `$currentDate`用来将*lastModified*字段设定为当前时间

  例如，先查找*item*为*paper*的文档，将此文档中*size*字段中的*uom*值更新为*cm*，并将*status*值更新为*p*

```python
db.inventory.update_one(
    {"item": "paper"},
    {"$set": {"size.uom": "cm", "status": "P"},
     "$currentDate": {"lastModified": True}})
```

进行一个循环

```python
loop = asyncio.get_event_loop()
loop.run_until_complete(do_update_one())
```

### 在inventory集合中更新多个文档

==使用`update_many()`命令==

例如，

```python
db.inventory.update_many(
    {"qty": {"$lt": 50}},
    {"$set": {"size.uom": "in", "status": "P"},
     "$currentDate": {"lastModified": True}})
```



## 删除数据

删除一个文档：==`delete_one()`==

删除多个文档：==`delete_many()`==





















