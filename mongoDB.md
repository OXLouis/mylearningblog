# mongoDB入门
### 实验环境
    通过以下命令：
        docker run  -d --name mongotest \
        -p 27017:27017 \
        -v ~\data\db:\data\db \
        mongo
创建了一个端口映射，同时通过数据卷实现可持久化的mongo微型服务器。
通过 以下命令将mongo添加到path中。<br>
export PATH=/usr/local/mongodb/bin:$PATH

## 基础概念
![](http://chuantu.biz/t6/338/1530758357x1822614020.png)
<br>
笔记：
* mongo中的collection 相当于sql中的表
* mongo中的document 相当于sql中的记录行
* field 相当于sql中的列
* mongo是非关系形数据库，不支持表连接（嵌入文档？）
* mongod相当于数据库服务，mongo相当于客户端
***

### 特殊数据库
* admin： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
* local: 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
* config: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

### 文档
文档是一组键值(key-value)对(即BSON)。MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 MongoDB 非常突出的特点。
<br>
关系型数据库的一张表类似C的数组，而mongoDB一个集合则更像python的列表。
<br>
注意事项：
1. 文档中的键/值对是有序的。
2. MongoDB区分类型和大小写。
3. MongoDB的文档不能有重复的键。
4. 文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。
5. 以下划线"_"开头的键是保留的(不是严格要求的)。

### 集合
集合就是 MongoDB 文档组，类似于 RDBMS （关系数据库管理系统：Relational Database Management System)中的表格。<br>
集合存在于数据库中，集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。
注意事项：
1. 集合名不能是空字符串""。
2. 集合名不能以"system."开头，这是为系统集合保留的前缀。

### capped collections
Capped collections 就是固定大小的collection。
<br>
它有很高的性能以及队列过期的特性(过期按照插入的顺序). 有点和 "RRD" 概念类似。
<br>
Capped collections是高性能自动的维护对象的插入顺序。它非常适合类似记录日志的功能 和标准的collection不同，你必须要显式的创建一个capped collection， 指定一个collection的大小，单位是字节。collection的数据存储空间值提前分配的。
<br>
要注意的是指定的存储大小包含了数据库的头信息。

### 元数据
数据库的信息是存储在集合中。它们使用了系统的命名空间：**dbname.system.**
<img src=http://chuantu.biz/t6/338/1530759996x1822614020.png />
<br>
笔记：
* 在indexes插入数据可以创建索引。
* users是可修改的。
* profile时可删除的。
* 其余文档不可变。

### MongoDB 数据类型
<img src=http://chuantu.biz/t6/338/1530760156x1822614026.png />

### ObjectID
ObjectId 类似唯一主键，可以很快的去生成和排序，包含 12 bytes，含义是：
1. 前 4 个字节表示创建 unix时间戳,格林尼治时间 UTC 时间，比北京时间早了 8 个小时
2. 接下来的 3 个字节是机器标识码
3. 紧接的两个字节由进程 id 组成 PID
4. 最后三个字节是随机数

### 基本操作
[MongoDB学习（四）常用操作命令](https://blog.csdn.net/qq_16313365/article/details/52313987)
[https://blog.csdn.net/qq_16313365/article/details/58599253](https://blog.csdn.net/qq_16313365/article/details/58599253)
    
### pymongo的基本操作
[pymongo基本操作](https://www.cnblogs.com/descusr/archive/2011/11/15/2249391.html)

#### 增
* db.createCollection(name, options) #直接向不存在集合插入也可以创建集合

* db.COLLECTION_NAME.insert(document) #document可以被定义为一个变量然后调用insert
* db.COLLECTION_NAME.save(document) #save类似insert，区别在于指定_id字段，则会更新对应id的数据
* db.COLLECTION_NAME.insertOne():向指定集合中插入一条文档数据
* db.COLLECTION_NAME.insertMany():向指定集合中插入多条文档数据

创建集合可选字段：
<img src=http://chuantu.biz/t6/338/1530770478x1822614020.png />
***
#### 改
    db.collection.update(
        <query1>,
        <update>,
        {
            upsert: <boolean>,
            multi: <boolean>,
            writeConcern: <document>
        }
    )
    
参数说明：
* query : update的查询条件，类似sql update查询内where后面的。
* update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
* upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
* multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
* writeConcern :可选，抛出异常的级别。

#### 删
    db.collection.remove(
        <query>,
        <justOne>
    )
2.6 版本后：

    db.collection.remove(
        <query>,
        {
            justOne: <boolean>,
            writeConcern: <document>
        }
    )

#### 查
    db.collection.find(query, projection)[.pretty()]
    加上pretty()可以返回易读的形式

条件操作符
* (>) 大于 - $gt
* (<) 小于 - $lt
* (>=) 大于等于 - $gte
* (<= ) 小于等于 - $lte

type操作符
<img src=http://chuantu.biz/t6/338/1530772652x1822614026.png />
<br>

find中and和or的用法：
```javascript
db.col.find({"likes": {$gt:50}, $or: [{"by": "菜鸟教程"},{"title": "MongoDB 教程"}]}).pretty()
```

#### limit与skip
limit: 用于限制返回结果数量。  
```javascript
db.COLLECTION_NAME.find().limit(NUMBER)
```
skip: 用于跳过指定数量的返回结果。
```javascript
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```
通过limit和skip可以实现分页。limit和skip其实通过游标实现。

#### MongoDB聚合
使用aggregate()方法中设置group参数实现聚合。  
示例：  
```javascript
db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}])
```
但aggregate()真正的作用是通过管道连接多层处理。  
![](https://img-blog.csdn.net/20160609100534149?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

每个阶段管道限制为100M的内存。几个常用的等级：
1. $project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
2. $match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
3. $limit：用来限制MongoDB聚合管道返回的文档数。
4. $skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
* $unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
* $group：将集合中的文档分组，可用于统计结果。
* $sort：将输入文档排序后输出。
* $geoNear：输出接近某一地理位置的有序文档。
* $out: 将结果输出到对应的collection中。必须是最后一个管道。
https://blog.csdn.net/congcong68/article/details/51620040

#### sort方法
指定参数，1升序，-1降序
```javascript
db.COLLECTION_NAME.find().sort({KEY:1})
```


### MongoDB索引
MongoDB使用 createIndex() 方法来创建索引。
MongoDB建造索引用的数据结构是B-树。
```javascript
db.collection.createIndex(keys, options)
```
设置多个字段创建索引可以创建复合索引。  
可选接受参数列表如下: 
<img src=http://chuantu.biz/t6/344/1531797192x-1566673375.png />   

#### 为什么MongoDB选择B-树
B+树内节点不存储数据，所有 data 存储在叶节点导致查询时间复杂度固定为 log n。而B-树查询时间复杂度不固定，与 key 在树中的位置有关，最好为O(1)  
我们说过，尽可能少的磁盘 IO 是提高性能的有效手段。MongoDB 是聚合型数据库，而 B-树恰好 key 和 data 域聚合在一起。  

#### 为什么Mysql使用B+树
Mysql 是一种关系型数据库，区间访问是常见的一种情况，而 B-树并不支持区间访问（可参见上图），而B+树由于数据全部存储在叶子节点，并且通过指针串在一起，这样就很容易的进行区间遍历甚至全部遍历。

###MongoDB 数组元素增删查改
[MongoDB 数组元素查询](https://blog.csdn.net/leshami/article/details/55049891)  
包括模糊匹配，精确匹配，返回指定文档。
数组中的$elemMatch和$all相当于离散数学中存在一个，和任意的关系。  
1. 数组查询有精确和模糊之分，精确匹配需要指定数据元素的全部值 
2. 数组查询可以通过下标的方式进行查询 
3. 数组内嵌套文档可以通过.成员的方式进行查询 
4. 数组至少一个元素满足所有指定的匹配条件可以使用$elemMatch 
5. 数组查询中返回元素的子集可以通过$slice以及占位符来实现6. all满足所有指定的匹配条件，不考虑多出的元素以及元素顺序问题
[MongoDB 数组元素增删改](https://blog.csdn.net/leshami/article/details/55192965)

###MongoDB 副本集
[MongoDB复制集原理](http://www.mongoing.com/archives/2155)


