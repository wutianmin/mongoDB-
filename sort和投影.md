## sort和投影

### 排序

*查询文档时，默认是按照_id值进行排序*

`sort({})`用来指定文档排序的规则

升序：`sort({salary:1})`

降序：`sort({salary:-1})`

例如，将员工集合按照工资从低到高排序

```
db.emp.find().sort({salary:1})
```

*如果`sort()`中传递两个参数，则表示先按照前者排序，前者数据相等时，再按照后者排序*

**`limit()`,`skip()`,`sort()`三者可以以任意的顺序进行调用**

### 投影
如果只想读取某字段的数据，在find()后添加参数
*默认_id都会显示*
```
db.collection.find({<query document>},{<projection document>})
```
`<field>:1` 读取该字段
`<field>:0` 不读取该字段

如，读取xx字段，且不显示_id
`find({},{xx:1,_id:0})`
