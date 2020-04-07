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

# 3 Connect to Your Cluster

...

+ import MongoClient from PyMongo
+ connet to your cluster

```python
from pymongo import MongoClient
client=MongoClient('mongodb+srv://MongoDBTMUser:wtm00198@helloworld-gjjka.mongodb.net/test?retryWrites=true&w=majority')
```

此时就建立好了一个client connection，并存储到了**client**变量中。

# 4 insert data into Atlas Cluster with the PyMongo Driver

+ 在cluster上建立一个名叫“gettingStarted”的新数据库，*db*变量指向这个新数据库。

```python
db=client.gettingStarted
```

+ 在gettingStarted数据库中创建一个新的名为“people”的集合，*people*变量指向这个新的集合。
```python
people=db.people
```

+ 创建一个文档。

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

+ 将此文档插入到“people”集合中。

```python
people.insert_one(personDocument)
```











