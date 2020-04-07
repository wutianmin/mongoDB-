# mongoDB

# 1 简介——构建数据

### (1) mongoDB中数据存储形式——JSON

在mongoDB中，数据以文档的形式存储。

这些文档以**JSON**(JavaScript Object Notation)格式存储在mongoDB中。

JSON文档支持嵌入式字段，因此相关数据和数据列表可以存储在文档中，而不是存储在外部表中。

==在JSON文档中，字段名和值之间用***冒号***分隔，每一对字段名和值之间用***逗号***分隔，字段集封装在***{}***中。==

例如：
| name     | quantity |
| -------- | -------- |
| notebook | 50       |
```mongoDB
{"name":"notebook","quantity":50}
```
### (2) 确定嵌入式数据并构建数据

当某一个字段有多项数值时，这些字段就是*嵌入文档*或*文档内嵌入文档*的列表/数组的候选字段。

例如：

size包含三个字段“高度、宽度、单位”

|       size       |
| :--------------: |
| $14\times 21$,cm |

```
{"h":21,"w":14,"uom":"cm"}
```

有些项目有多个评级，因此评级可以表示为*包含得分字段的**list***

| rating |
| :----: |
|   8    |

```
[{"score":9},{"score":8}]
```

如果一个项目中有多个同等地位的值，将它们存储在列表中

|           tags           |
| :----------------------: |
| college-ruled,perforated |

```
["college-ruled","perforated"]
```

| name     | quantity | size              | status | tags                     | rating |
| -------- | -------- | ----------------- | ------ | ------------------------ | ------ |
| notebook | 50       | $8.5\times 11$,in | A      | college-ruled,perforated | 8      |

```
{
"name":"notebook",
"qty":50,
"rating":[{"score":8},{"score":9}],
"size":{"h":11,"w":8.5,"unit":"in"},
"status":"A",
"tags":["college-ruled","perforated"]
}
```

# 2 Get Started with Atlas[¶](https://docs.atlas.mongodb.com/getting-started/#get-started-with-atlas)

账户：597190990@qq.com

mongoDBuser:mongoDBTMUser

# 3 MongoDB 在python端的操作

## 建立连接

+ import MongoClient from PyMongo
+ connet to your cluster

```python
from pymongo import MongoClient
client=MongoClient('mongodb+srv://MongoDBTMUser:wtm00198@helloworld-gjjka.mongodb.net/test?retryWrites=true&w=majority')
```

此时就建立好了一个client connection，并存储到了**client**变量中。

## 插入数据

+ 步骤

（1）在cluster上建立一个名叫“gettingStarted”的新database，*db*变量指向这个新database

```python
db=client.gettingStarted
```

（2）在gettingStarted数据库中创建一个新的名为“people”的collection，*people*变量指向这个新的collection

```python
people=db.people
```

（3）创建一个document

```python
import datetime
personDocument={
    "name":{"first":"Alan","last":"Turing"},
"birth":datetime.datetime(1912,6,23),
"death":datetime.datetime(1954,6,7),
"contribs":["Turing machine","Turing test","Turingery"],
"views":125000
    }
```

这个文档存储在*personDocument*变量中。

（4）当插入一个document时，使用命令*insert_one*将此文档插入到“people”集合中

```python
people.insert_one(personDocument)
```

##### 例如：

```python
db=client.test
>>> db.inventory.insert_one(
	{"item":"canvas",
	 "qty":100,
	 "tags":["cotton"],
	 "size":{"h":28,"w":35.5,"uom":"cm"}})
```

或

```python
db=client.test
>>> coll=db.inventory
>>> coll.insert_one({"item":"canvas",
	 "qty":100,
	 "tags":["cotton"],
	 "size":{"h":28,"w":35.5,"uom":"cm"}})
```

得到如下结果：

<pymongo.results.InsertOneResult object at 0x00000214528BCA00>

即插入数据成功。

## 显示数据

```python
>>> cursor=db.inventory.find({})
>>> from pprint import pprint
>>> for inventory in cursor:
	pprint(inventory)
```

得到如下结果：

{'_id': ObjectId('5e8c35ed7a705ec009a9466f'),
 'item': 'canvas',
 'qty': 100,
 'size': {'h': 28, 'uom': 'cm', 'w': 35.5},
 'tags': ['cotton']}

## 检索数据

### 导入数据并查询数据 

首先，用命令*insert_many*插入多项数据

```python
＃在这些示例中，子文档的键顺序很重要，因此我们必须
＃使用bson.son.SON而不是Python dict
from bson.son import SON
db.inventory.insert_many([
    {"item": "journal",
     "qty": 25,
     "size": SON([("h", 14), ("w", 21), ("uom", "cm")]),
     "status": "A"},
    {"item": "notebook",
     "qty": 50,
     "size": SON([("h", 8.5), ("w", 11), ("uom", "in")]),
     "status": "A"},
    {"item": "paper",
     "qty": 100,
     "size": SON([("h", 8.5), ("w", 11), ("uom", "in")]),
     "status": "D"},
    {"item": "planner",
     "qty": 75,
     "size": SON([("h", 22.85), ("w", 30), ("uom", "cm")]),
     "status": "D"},
    {"item": "postcard",
     "qty": 45,
     "size": SON([("h", 10), ("w", 15.25), ("uom", "cm")]),
     "status": "A"}])
```

在这个数据库中查询`status`为`D`的文档

```python
cursor = db.inventory.find({"status": "D"})
from pprint import pprint
for inventory in cursor:
	pprint(inventory)
```

就可以显示所有`status`为`D`的文档

{'_id': ObjectId('5e8c3af1140f53b77559b4ca'),
 'item': 'paper',
 'qty': 100,
 'size': {'h': 8.5, 'uom': 'in', 'w': 11},
 'status': 'D'}
{'_id': ObjectId('5e8c3af1140f53b77559b4cb'),
 'item': 'planner',
 'qty': 75,
 'size': {'h': 22.85, 'uom': 'cm', 'w': 30},
 'status': 'D'}

### 以嵌入文档为标准查询数据

+ 根据嵌入式文档的内容检索集合中的特定文档

==如果希望选择与嵌入式JSON对象中所有字段匹配的文档==，请指定嵌入式文档的完全匹配项**，包括该嵌入式文档中所有按照它们在集合中出现的顺序的字段和值。

查询`size`字段等于文档`{ "h" : 14, "w" : 21, "uom" : "cm"}`的所有文档

```python
cursor = db.inventory.find(
    {"size": SON([("h", 14), ("w", 21), ("uom", "cm")])})
from pprint import pprint
for inventory in cursor:
     pprint(inventory)
```

得到如下结果

{'_id': ObjectId('5e8c3af1140f53b77559b4c8'),
 'item': 'journal',
 'qty': 25,
 'size': {'h': 14, 'uom': 'cm', 'w': 21},
 'status': 'A'}

+ 用点表示法(dot notation)检索集合中的特定文档

==如果希望选择仅与嵌入式JSON对象中的字段之一完全匹配的文档==

**进行查询时，字段名称和嵌套字段名称必须在引号内**

例如，查询所有`size`字段中`uom`为`in`的document

```python
cursor = db.inventory.find({"size.uom": "in"})
from pprint import pprint

for inventory in cursor:
     pprint(inventory)
```





































































