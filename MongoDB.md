# MongoDB笔记

# 1:概念场景和术语

## 1.1:关系型数据库

传统的关系型数据库(mysql)，在数据操作的“三高”需求以及应对Web2.0的网站需求面前，显得力不从心

解释三高的需求：

• High performance - 对数据库高并发读写的需求

• Huge Storage - 对海量数据的高效率存储和访问的需求

• High Scalability && High Availability- 对数据库的高可扩展性和高可用性的需求



而MongoDB可应对三高需求



## 1.2:应用场景

具体的应用场景如： 

1）社交场景，使用 MongoDB 存储存储用户信息，以及用户发表的朋友圈信息，通过地理位置索引实现附近的人、地点等功能

2）游戏场景，使用 MongoDB 存储游戏用户信息，用户的装备、积分等直接以内嵌文档的形式存储，方便查询、高效率存储和访问

3）物流场景，使用 MongoDB 存储订单信息，订单状态在运送过程中会不断更新，以 MongoDB 内嵌数组的形式来存储，一次查询就能将 订单所有的变更读取出来

4）物联网场景，使用 MongoDB 存储所有接入的智能设备信息，以及设备汇报的日志信息，并对这些信息进行多维度的分析

5）视频直播，使用 MongoDB 存储用户信息、点赞互动信息等



这些应用场景中，数据操作方面的共同特点是： 

1）数据量大 

2）写入操作频繁（读写都很频繁）

3）价值较低的数据，对事务性要求不高 

对于这样的数据，我们更适合使用MongoDB来实现数据的存储



## 1.3:什么时候选择它

在架构选型上，除了上述的三个特点外，如果你还犹豫是否要选择它？可以考虑以下的一些问题： 

1：应用不需要事务及复杂 join 支持 

2：新应用，需求会变，数据模型无法确定，想快速迭代开发 

3：应用需要2000-3000以上的读写QPS（更高也可以） 

4：应用需要TB甚至 PB 级别数据存储 

5：应用发展迅速，需要能快速水平扩展 

6：应用要求存储的数据不丢失 

7：应用需要99.999%高可用 

8：应用需要大量的地理位置查询、文本查询 



如果上述有1个符合，可以考虑 MongoDB，2个及以上的符合，选择 MongoDB 绝不会后悔

相对MySQL，面对以上的场景，可以用更低的成本解决问题



## 1.4:MongoDB简介

MongoDB是一个开源、高性能、`无模式`的文档型数据库，当初的设计就是用于简化开发和方便扩展，是

NoSQL数据库产品中的一种。是最像关系型数据库（MySQL）的非关系型数据库

无模式：就是没有像Mysql的具体的列来约束它，就是无模式，比较松散，容易扩展

它支持的数据结构非常松散，是一种类似于 JSON 的 格式叫BSON

所以它既可以存储比较复杂的数据类型，又相当的灵活

MongoDB中的记录是一个文档，它是一个由字段和值对（field:value）组成的数据结构

MongoDB文档类似于JSON对象，即`一个文档认为就是一个对象`。字段的数据类型是字符型

它的值除了使用基本的一些类型外，还可以包括其他文档、普通数组和文档数组



## 1.5:体系结构

![image-20221008211600897](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221008211600897.png)

## 1.6: 数据模型

MongoDB的最小存储单位就是文档(document)对象。文档(document)对象对应于关系型数据库的行。数据在MongoDB中以 BSON（Binary-JSON）文档的格式存储在磁盘上



## 1.7:特点

MongoDB主要有如下特点： 

1）高性能： MongoDB提供高性能的数据持久性。特别是, 对嵌入式数据模型的支持减少了数据库系统上的I/O活动。 索引支持更快的查询，并且可以包含来自嵌入式文档和数组的键。（文本索引解决搜索的需求、TTL索引解决历史数据自动过期的需求、地 理位置索引可用于构建各种 O2O 应用） mmapv1、wiredtiger、mongorocks（rocksdb）、in-memory 等多引擎支持满足各种场景需求。 Gridfs解决文件存储的需求

2）高可用性： MongoDB的复制工具称为副本集（replica set），它可提供自动故障转移和数据冗余

3）高扩展性： MongoDB提供了水平可扩展性作为其核心功能的一部分。 分片将数据分布在一组集群的机器上。（海量数据存储，服务能力水平扩展） 从3.4开始，MongoDB支持基于片键创建数据区域。在一个平衡的集群中，MongoDB将一个区域所覆盖的读写只定向到该区域内的那些 片

4）丰富的查询支持： MongoDB支持丰富的查询语言，支持读和写操作(CRUD)，比如数据聚合、文本搜索和地理空间查询等

5）其他特点：如无模式（动态模式）、灵活的文档模型



# 2:环境配置

## 2.1:安装

1：下载ZIP，解压

2：配置环境变量

3：在解压目录中，手动建立一个目录用于存放数据文件，如 data/db



## 2.2:启动

启动方式1：

命令行参数方式启动服务 在 bin 目录中打开命令行提示符，输入如下命令

```apl
mongod --dbpath=..\data\db
```

我们在启动信息中可以看到，mongoDB的默认端口是27017

如果我们想改变默认的启动端口，可以通过--port来指定端口



启动方式2：

使用配置文件方式启动服务

在解压目录中新建 conf 文件夹，该文件夹中新建配置文件 mongod.conf ，内如参考如下：

```apl
dbpath=E:\Tools\mongodb\data\db
```

更多的配置可自行百度

启动命令如下，二选一：

```apl
mongod -f ../conf/mongodb.conf
或者
mongod --config ../conf/mongodb.conf
```



## 2.3:连接

直接在bin目录下面执行下面的命令

```apl
mongo
或者
mongo --host=127.0.0.1 --port=27017
```



## 2.4:Linux安装

1：上传

2：解压到`/usr/local/mongodb`

3：创建数据库目录和日志目录

```
mkdir -p data/db
mkdir logs
touch /usr/local/mongodb/logs/mongodb.log
```

4：配置环境变量

5：配置文件配置如下

```
#指定数据库路径
dbpath=/usr/local/software/mongdb/data/db
#指定MongoDB日志文件
logpath=/usr/local/software/mongdb/logs/mongodb.log
# 使用追加的方式写日志
logappend=true
#端口号
port=27017 
#方便外网访问
bind_ip=0.0.0.0
fork=true # 以守护进程的方式运行MongoDB，创建服务器进程
#auth=true #启用用户验证
#bind_ip=0.0.0.0 #绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定则默认本地所有IP
```

6：启动

```
mongod -f /etc/mongodb.conf
```

7：关闭

```
mongod --shutdown -f /etc/mongodb.conf
```



# 3:基本命令

## 3.1:案例需求

存放文章评论的数据存放到MongoDB中，数据结构参考如下：

![image-20221009103955778](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009103955778.png)

## 3.2:数据库操作

### 1:创建数据库

选择和创建数据库的语法格式：`use 数据库名称`

如果数据库不存在则自动创建，例如，以下语句创建 spitdb 数据库：`use articledb`

注意：在 MongoDB 中，集合只有在内容插入后才会创建! 

就是说：创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建

我们第一次创建的时候，例如`use articledb`

实际上只存在内存，还没有到磁盘，当第一次插入数据了，就会持久化到磁盘了



### 2:展示数据库

查看有权限查看的所有的数据库命令：`show dbs`



### 3:查看当前使用的数据库

查看当前正在使用的数据库命令:`db`

MongoDB 中默认的数据库为 test，如果你没有选择数据库，集合将存放在 test 数据库中

有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库

- admin： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器
- local: 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合 
- config: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息



### 4:数据库的删除

MongoDB 删除数据库的语法格式如下： use 数据库名称，例如：`db.dropDatabase()`

提示：用来删除已经持久化的数据库，删除持久化的才有用，不然无效



### 5:小结数据库操作

![image-20221010155138032](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010155138032.png)



## 3.2:集合操作

集合类似于数据库的表

### 1:集合的显式创建

创建语法：`db.createCollection(name)`

name: 要创建的集合名称



### 2:查看当前库中的集合

也就是类似于查询数据库里面的表

两个命令：`show collections` 或 `show tables`



### 3:集合隐式创建

当向一个集合中插入一个文档的时候，如果集合不存在，则会自动创建集合



### 4:集合的删除

集合的删除`db.collection.drop()`



### 5:集合操作小结

![image-20221010160531916](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010160531916.png)

## 3.3:文档操作

文档（document）的数据结构和 JSON 基本一样。 所有存储在集合中的数据都是 BSON 格式

### 1:单个文档插入

使用insert() 或 save() 方法向集合中插入文档

```
db.collection.insertOne(
	<document or array of documents>,
	{
		writeConcern: <document>,
		ordered: <boolean>
	}
)
```

主要就关注第一行的文档内容，下面一般不管

![image-20221009111153445](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009111153445.png)

要向comment的集合(表)中插入一条测试数据：

注意：插入数据的时候，默认数字都是double类型，使用整数要加上`NumberInt()`

```
db.comment.insertOne(
	{
		"articleid":"100000",
		"content":"今天天气真好，阳光明媚",
		"userid":"1001",
		"nickname":"Rose",
		"createdatetime":new Date(),
		"likenum":NumberInt(10),
		"state":null
	}
)
```

会自动创建出一个叫做 `comment`的集合

1）comment集合如果不存在，则会隐式创建 

2）mongo中的数字，默认情况下是double类型，如果要存整型，必须使用函数NumberInt(整型数字)，否则取出来就有问题了

3）插入当前日期使用 new Date() 

4）插入的数据没有指定 _id ，会自动生成主键值 

5）如果某字段没值，可以赋值为null，或不写该字段。



注意： 

1. 文档中的键/值对是有序的
2. 文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)
3. MongoDB区分类型和大小写
4. MongoDB的文档不能有重复的键
5. 文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符



### 2:批量文档插入

```
db.collection.insertMany(
	[ <document 1> , <document 2>, ... ],
	{
		writeConcern: <document>,
		ordered: <boolean>
	}
)
```

主要就关注第一行的文档内容，下面一般不管

![image-20221009112348652](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009112348652.png)

批量插入多条文章评论：

```
db.comment.insertMany([
{
	"_id":"1",
	"articleid":"100001",
	"content":"我们不应该把清晨浪费在手机上，健康很重要，一杯温水幸福你我他",
	"userid":"1002",
	"nickname":"相忘于江湖",
	"createdatetime":new Date("2019-08-05T22:08:15.522Z"),
	"likenum":NumberInt(1000),
	"state":"1"
},
{
	"_id":"2",
	"articleid":"100001",
	"content":"我夏天空腹喝凉开水，冬天喝温开水",
	"userid":"1005",
	"nickname":"伊人憔悴",
	"createdatetime":new Date("2019-08-05T23:58:51.485Z"),
	"likenum":NumberInt(888),
	"state":"1"
},
{
	"_id":"3",
	"articleid":"100001",
	"content":"我一直喝凉开水，冬天夏天都喝",
	"userid":"1004",
	"nickname":"杰克船长",
	"createdatetime":new Date("2019-08-06T01:05:06.321Z"),
	"likenum":NumberInt(666),
	"state":"1"
},
{
	"_id":"4",
	"articleid":"100001",
	"content":"专家说不能空腹吃饭，影响健康",
	"userid":"1003",
	"nickname":"凯撒",
	"createdatetime":new Date("2019-08-06T08:18:35.288Z"),
	"likenum":NumberInt(2000),
	"state":"1"
},
{
	"_id":"5",
	"articleid":"100001",
	"content":"研究表明，刚烧开的水千万不能喝，因为烫嘴",
	"userid":"1003",
	"nickname":"凯撒",
	"createdatetime":new Date("2019-08-06T11:01:02.521Z"),
	"likenum":NumberInt(3000),
	"state":"1"
}
]);
```

提示： 

插入时指定了 _id ，则主键就是该值

如果某条数据插入失败，将会终止插入，但已经插入成功的数据不会回滚掉



### 3:文档的基本查询

语法`db.collection.find(<query>, [projection])`

主要就关注文档内容，后面一般不管

![image-20221009112959326](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009112959326.png)

这里你会发现每条文档会有一个叫_id的字段，这个相当于我们原来关系数据库中表的主键，当你在插入文档记录时没有指定该字段， MongoDB会自动创建，其类型是ObjectID类型。 如果我们在插入文档记录时指定该字段也可以，其类型可以是ObjectID类型，也可以是MongoDB支持的任意类型



1：查询某个库的集合的全部文档：`db.comment.find()`或者`db.comment.find({})`



2：条件查询

语法：`db.comment.find({userid:'1003'})`



3：查询一个

如果你只需要返回符合条件的第一条数据，我们可以使用findOne命令来实现，语法和find一样

语法：`db.comment.findOne({userid:'1003'})`



### 4:投影查询

如果要查询结果返回部分字段，则需要使用投影查询（不显示所有字段，只显示指定的字段）

如：查询结果只显示 _id、userid、nickname 

```
db.comment.find({userid:"1003"},{userid:1,nickname:1})
```

{userid:1,nickname:1}代表会显示

默认 _id 会显示。 如：查询结果只显示 、userid、nickname ，不显示 _id ：

```
db.comment.find({userid:"1003"},{userid:1,nickname:1,_id:0})
```



### 5:文档的覆盖更新

```
db.collection.updateOne(
	<query>,
	<update>,
	{
		upsert: <boolean>,
		multi: <boolean>,
		writeConcern: <document>,
		collation: <document>,
		arrayFilters: [ <filterdocument1>, ... ],
		hint: <document|string>
	}
)
```

主要关注前面两个参数，后面一般不管，需要再加

覆盖的修改 如果我们想修改_id为1的记录，点赞量为1001，输入以下语句

`db.comment.update({_id:"1"},{likenum:NumberInt(1001)})`

这个的效果实际上是用`likenum:NumberInt(1001)`替换了所有的文档内容，并不单单更新了 likenum的内容

类似于先删除再执行插入



### 6:文档的局部更新

为了解决这个问题，我们需要在update的前面加上$set来实现，命令如下

我们想修改_id为2的记录，浏览量为889，输入以下语句：

`db.comment.updateOne({_id:"2"},{$set:{likenum:NumberInt(889)}})`

这样其他的字段就不会发生变化了



### 7:批量更新

更新所有用户为 1003 的用户的昵称为 凯撒大帝 

```
//修改所有符合条件的数据
db.comment.updateMany(
    {articleid:'100001'},
    {$set:{nickname:"凯撒大帝"}}
)
```



### 8:列值增长的更新

如果我们想实现对某列值在原有值的基础上进行增加或减少，可以使用 $inc 运算符来实现，可用于点赞

需求：对3号数据的点赞数，每次递增1

```
db.comment.update({_id:"3"},{$inc:{likenum:NumberInt(1)}})
```



### 9:删除文档

删除文档的语法结构：`db.集合名称.deleteOne(条件)`

可以将数据全部删除`db.comment.deleteMany({})`，请慎用

如果删除_id=1的记录，输入以下语句

`db.comment.removeOne({_id:"1"})`



### 10:文档基本操作小结

![image-20221010163515192](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010163515192.png)

![image-20221010163545788](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010163545788.png)

![image-20221010163604854](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010163604854.png)

![image-20221010163625812](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010163625812.png)

![image-20221010163639467](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010163639467.png)



## 3.4:文档的分页查询

### 1:统计查询count

统计查询使用countDocuments()方法，语法如下：

```
db.collection.countDocuments(query, options)
```

![image-20221009162350726](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009162350726.png)

统计comment集合的所有的记录数：

```
db.comment.countDocuments()
```



统计userid为1003的记录条数

```
db.comment.countDocuments({userid:"1003"})
```



默认情况下 countDocuments() 方法返回符合条件的全部记录条数



### 2:分页列表查询limit

可以使用limit()方法来读取指定数量的数据，使用skip()方法来跳过指定数量的数据，分页是这两种一起完成

如果你想返回指定条数的记录，可以在find方法后调用limit来返回结果，limit的默认值20

```
db.comment.find().limit(3)
```

skip方法同样接受一个数字参数作为跳过的记录条数。（前N个不要），默认值是0

```
db.comment.find().skip(3)
```

分页一般配合两个标签一起使用

一共五条数据，每页两条，查询第二页怎么查

```
db.comment.find().limit(2).skip(2)
```



```
//第一页
db.comment.find().skip(0).limit(2)
//第二页
db.comment.find().skip(2).limit(2)
//第三页
db.comment.find().skip(4).limit(2)
```

总结：

分页：`db.comment.find.skip( (pageNum-1)*pageSize ).limit( pageSize )`



### 3:排序查询

sort() 方法对数据进行排序，sort() 方法可以通过参数指定排序的字段

并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而 -1 是用 于降序排列

```
db.集合名称.find().sort(排序方式)
```

例如：对userid降序排列，并对访问量进行升序排列

```
db.comment.find().sort({userid:-1,likenum:1})
```

skip(), limilt(), sort()三个放在一起执行的时候

执行的顺序是先 sort()，然后是 skip()，最后是显示的 limit()，和命令编写顺序无关



### 4:小结

![image-20221010164403993](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010164403993.png)

## 3.5:文档的更多查询

### 1:正则的复杂条件查询

提示：正则表达式是js的语法，直接写的写法

例如：我要查询评论内容包含“开水”的所有文档，代码如下：

```
db.comment.find({content:/开水/})
```

例如：如果要查询评论的内容中以“专家”开头的，代码如下：

```
db.comment.find({content:/^专家/})
```



### 2:比较查询

<，<=，>，>= 这个操作符也是很常用的，格式如下：

```
db.集合名称.find({ "field" : { $gt: value }}) // 大于: field > value
db.集合名称.find({ "field" : { $lt: value }}) // 小于: field < value
db.集合名称.find({ "field" : { $gte: value }}) // 大于等于: field >= value
db.集合名称.find({ "field" : { $lte: value }}) // 小于等于: field <= value
db.集合名称.find({ "field" : { $ne: value }}) // 不等于: field != value
```

示例：查询评论点赞数量大于700的记录

```
db.comment.find({likenum:{$gt:NumberInt(700)}})
```



### 3:包含查询

类似于Mysql的in语法，使用$in操作符

示例：查询评论的集合中userid字段包含1003或1004的文档

```
db.comment.find({userid:{$in:["1003","1004"]}})
```



不包含使用$nin操作符

示例：查询评论集合中userid字段不包含1003和1004的文档

```
db.comment.find({userid:{$nin:["1003","1004"]}})
```



### 4:条件连接查询

我们如果需要查询同时满足两个以上条件，需要使用$and操作符将条件进行关联

格式为：

```
$and:[ { },{ },{ } ]
```

示例：查询评论集合中likenum大于等于700 并且小于2000的文档：

```
db.comment.find({$and:[{likenum:{$gte:NumberInt(700)}},{likenum:{$lt:NumberInt(2000)}}]})
```

如果两个以上条件之间是或者的关系，我们使用 操作符进行关联，与前面 and的使用方式相同 

格式为：

```
$or:[ { },{ },{ } ]
```

示例：查询评论集合中userid为1003，或者点赞数小于1000的文档记录

```
db.comment.find({$or:[ {userid:"1003"} ,{likenum:{$lt:1000} }]})
```









### 5:小结

![image-20221010165648235](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010165648235.png)

![image-20221010165702653](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010165702653.png)

## 3.6:总结

```apl
选择切换数据库：use articledb

插入一条数据：db.comment.insertOne({bson数据})

插入多条数据：db.comment.insertMany({bson数据},{bson数据}....)

查询所有数据：db.comment.find()

条件查询数据：db.comment.find({条件})

查询符合条件的第一条记录：db.comment.findOne({条件})

查询符合条件的前几条记录：db.comment.find({条件}).limit(条数)

查询符合条件的跳过的记录：db.comment.find({条件}).skip(条数)

全量修改一条数据：db.comment.updateOne({条件},{修改后的数据}) 

局部修改一条数据：db.comment.updateOne({条件},{$set:{要修改部分的字段:数据})

修改全部符合条件的数据：db.comment.updateMany({条件},{修改后的数据}) 

修改数据并自增某字段值：db.comment.update({条件},{$inc:{自增的字段:步进值}})

删除第一条数据：db.comment.deleteOne({条件})

删除符合条件全部数据：db.comment.deleteMany({条件})

统计查询：db.comment.countDocuments({条件})

模糊查询：db.comment.find({字段名:/正则表达式/})

条件比较运算：db.comment.find({字段名:{$gt:值}})

包含查询：db.comment.find({字段名:{$in:[值1，值2]}})

不包含查询：db.comment.find({字段名:{$nin:[值1，值2]}})

条件连接查询and：db.comment.find({$and:[{条件1},{条件2}]})

条件连接查询or：db.comment.find({$or:[{条件1},{条件2}]})
```



# 4:索引

## 4.1:概述

索引支持在MongoDB中高效地执行查询。如果没有索引，MongoDB必须执行全集合扫描，即扫描集合中的每个文档，以选择与查询语句匹配的文档。这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的



索引是特殊的数据结构，它以易于遍历的形式存储集合数据集的一小部分。索引存储特定字段或一组字段的值，按字段值排序。索引项的排序支持有效的相等匹配和基于范围的查询操作，此外，MongoDB还可以使用索引中的排序返回排序结果



Mysql使用的是B+树

MongoDB使用的是B树



## 4.2:单字段索引

MongoDB支持在文档的单个字段上创建用户定义的升序/降序索引，称为单字段索引

对于单个字段索引和排序操作，索引键的排序顺序并不重要，因为MongoDB可以在任何方向上遍历索引

![image-20221009165838347](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009165838347.png)

先查询索引结构的30，直接定位到集合的元素



## 4.3:复合索引

MongoDB还支持多个字段的用户定义索引，即复合索引（Compound Index）

复合索引中列出的字段顺序具有重要意义，例如，如果复合索引由 { userid: 1, score: -1 } 组成，则索引首先

按userid正序排序，然后 在每个userid的值内，再在按score倒序排序

![image-20221009170109249](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009170109249.png)

## 4.4:其他索引

地理空间索引、文本索引、哈希索引、 地理空间索引 ，了解即可



## 4.5:索引的查看

返回一个集合中的所有索引的数组

```
db.collection.getIndexes()
```

示例：查看comment的所有索引

```
db.comment.getIndexes()
```

结果：

```
[
    {
        "v" : 2.0, 
        "key" : {
            "_id" : 1.0
        }, 
        "name" : "_id_"
    }
]
```

- v：索引版本号
- key：哪些字段用了索引
- name：索引的名称



结果中显示的是默认` _id `索引。 默认`_id`索引： MongoDB在创建集合的过程中，在` _id `字段上创建一个

唯一的索引，默认名字为` _id_` ，该索引可防止客户端插入两个具有相同值的文 档，您不能在`_id`字段上

删除此索引，该索引是唯一索引，因此值不能重复，即 _id 值不能重复的

在分片集群中，通常使用 _id 作为片键

## 4.6:索引的创建

在集合上创建索引。语法：

```
db.collection.createIndex(keys, options)
```

一般都不写可选项options，看自己需求，需要则在查

![image-20221009171808867](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009171808867.png)



options的可选项：

![image-20221009171831324](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009171831324.png)

### 1:单字段索引

示例：对 userid 字段建立索引：

```
db.comment.createIndex({userid:1})
```

参数1：按升序创建索引

查看一下：

```
[
    {
        "v" : 2.0, 
        "key" : {
            "_id" : 1.0
        }, 
        "name" : "_id_"
    }, 
    {
        "v" : 2.0, 
        "key" : {
            "userid" : 1.0
        }, 
        "name" : "userid_1"
    }
]
```



### 2:复合索引

复合索引：对 userid 和 nickname 同时建立复合（Compound）索引：

```
db.comment.createIndex({userid:1,nickname:-1})
```

查看一下：

```
[
    {
        "v" : 2.0, 
        "key" : {
            "_id" : 1.0
        }, 
        "name" : "_id_"
    }, 
    {
        "v" : 2.0, 
        "key" : {
            "userid" : 1.0
        }, 
        "name" : "userid_1"
    }, 
    {
        "v" : 2.0, 
        "key" : {
            "userid" : 1.0, 
            "nickname" : -1.0
        }, 
        "name" : "userid_1_nickname_-1"
    }
]
```



## 4.7:索引的移除

### 1:根据条件删除

语法：

```
db.collection.dropIndex(index)
```

![image-20221009172336641](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009172336641.png)

删除 comment 集合中 userid 字段上的升序索引：

```
db.comment.dropIndex({userid:1})
```

再查看一下：发现少了一个

```
[
    {
        "v" : 2.0, 
        "key" : {
            "_id" : 1.0
        }, 
        "name" : "_id_"
    }, 
    {
        "v" : 2.0, 
        "key" : {
            "userid" : 1.0, 
            "nickname" : -1.0
        }, 
        "name" : "userid_1_nickname_-1"
    }
]
```



### 2:删除所有

```
db.collection.dropIndexes()
```

注意：`_id_`这个索引是不会被删除的



## 4.8:索引的使用

### 1:执行计划

分析查询性能（Analyze Query Performance）通常使用执行计划（解释计划、Explain Plan）来查看查询的情况，如查询耗费的时间、是 否基于索引查询等。 那么，通常，我们想知道，建立的索引是否有效，效果如何，都需要通过执行计划查看，其实发现跟mysql非常类似，一般用在查询语句上



语法：

```
db.collection.find(query,options).explain(options)
```

options一般可以不写，需要再写



案例：

查看根据userid查询数据的执行情况：

```
db.comment.find({userid:"1003"}).explain()
```

查看结果：

![image-20221009173507641](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009173507641.png)

主要看这个云计划下的stage字段，是否使用索引

- COLLSCAN：就是逐条扫描，没有索引
- FETCH：就是使用了索引



### 2:涵盖的查询

类似Mysql的回表查询，也就是比如`userid`存在索引，`comment`不存在索引

只查询userid或者_id的时候，就叫做涵盖的查询



## 4.9:小结

![image-20221010170542045](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221010170542045.png)









# 5:CRUD

## 0:springboot整合

1：依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```



2：配置文件

```yml
spring:
  data:
    mongodb:
      host: 192.168.61.133
      port: 27017
      database: articledb
```

也可以使用uri的方式来指定，就跟mysql一样，但是无安全验证

## 5.1:表结构分析

![image-20221009174205869](https://zzx-note.oss-cn-beijing.aliyuncs.com/mongodb/image-20221009174205869.png)



## 5.2:技术选型

1：mongodb-driver【了解】

mongodb-driver是mongo官方推出的java连接mongoDB的驱动包，相当于JDBC驱动



2：SpringDataMongoDB 

SpringData家族成员之一，用于操作MongoDB的持久层框架，封装了底层的mongodb-driver，现在都用这个



## 5.3:注解+实体类

```java
@Data
@Document(collection="comment")//可以省略，如果省略，则默认使用类名小写映射集合
public class Comment implements Serializable {
    private static final long serialVersionUID = 5456760893113311901L;

    //主键，如果属性就叫做id，则可以不写，默认就把id作为主键
    @Id
    private String id;

    //该属性对应mongodb的字段的名字，如果一致，则无需该注解
    @Field("content")
    private String content;//吐槽内容
    private Date publishtime;//发布日期

    //添加了一个单字段的索引
    @Indexed
    private String userid;//发布人ID

    private String nickname;//昵称
    private LocalDateTime createdatetime;//评论的日期时间
    private Integer likenum;//点赞数
    private Integer replynum;//回复数
    private String state;//状态
    private String parentid;//上级ID
    private String articleid;

}
```

- @Document：声明这个实体类为MongoDB的文档
- @Document可以使用collection属性指定文档名称，不写则默认为类名小写作为集合名称
- @Id：声明这个属性作为MongoDB的文档ID，如果属性为id，则不写@Id也默认把属性id作为主键
- @Field：属性和文档字段不一致就可以使用，一样就不需要声明了
- @Indexed：添加这个字段的索引
- @CompoundIndex( def = "{'userid': 1, 'nickname': -1}")，类注解，添加复合索引
- @Transient：映射忽略的字段，该字段不会保存到mongodb，只作为普通的`javaBean`属性。



## 5.4:使用API

利用的是`MongoTemplate`

### 1:创建集合

```java
@Test
public void test01(){
    boolean comment = mongoTemplate.collectionExists("comment");
    log.info("集合是否存在？{}",comment);
    if(!comment){
        mongoTemplate.createCollection("comment");
        log.info("创建comment集合成功");
    }
}
```

### 2:删除集合

```java
@Test
public void test02(){
    mongoTemplate.dropCollection("comment");
    log.info("删除集合成功");
}
```

### 3:增加文档

```java
@Test
public void test03(){
    Comment comment=new Comment();
    comment.setArticleid("100000");
    comment.setContent("测试添加的数据");
    comment.setCreatedatetime(LocalDateTime.now());
    comment.setUserid("1003");
    comment.setNickname("凯撒大帝");
    comment.setState("1");
    comment.setLikenum(0);
    comment.setReplynum(0);
    mongoTemplate.insert(comment);
}
@Test
public void test04(){
    Comment comment=new Comment();
    comment.setArticleid("100000");
    comment.setContent("测试添加的数据");
    comment.setCreatedatetime(LocalDateTime.now());
    comment.setUserid("1003");
    comment.setNickname("凯撒大帝");
    comment.setState("1");
    comment.setLikenum(0);
    comment.setReplynum(0);
    mongoTemplate.save(comment);
}
```

注意到：insert和save都可以插入文档，区别在哪儿？

- insert可以批量插入文档，重复id会报错
- save不可以批量插入数据，重复id不会报错



### 4:查询文档

```java
@Test
public void test05(){
    //查所有
    List<Comment> all = mongoTemplate.findAll(Comment.class);
    
    //根据id查询
    Comment comment = mongoTemplate.findById("6343e6c07487fa12c6ec758f", Comment.class);
}
```



### 5:Query对象

这个对象是Spring提供的，主要是为了查询准备的一个参数，可以封装查询之后的结果处理条件

- 排序
- 分页
- 总条数
- 去重

这个Query对象的addCriteria方法，添加一个Criteria对象，还可以封装查询的条件

Query对象有一个静态方法`query(Criteria criteria)`可以直接创建出一个Query对象



### 6:Criteria对象

用于封装查询条件的一个对象

- 等值查询
- 大于等于，小于等于
- and
- or
- 等等

`Criteria`直接调用对应的那些添加条件的静态方法，可以直接返回一个 Criteria 的对象

所以，可以这样用：

```
@Test
public void test07(){
    Query query=new Query();
    query.addCriteria(Criteria.where("userid").is("1002"));
    List<Comment> comments = mongoTemplate.find(query,Comment.class);
    log.info("数据：{}",comments);
}
```

也可以这样用：

```java
@Test
public void test07(){
    List<Comment> userid = mongoTemplate.find(Query.query(
    Criteria.where("userid").is("1003")), Comment.class);
}
```



### 7:示例

```java
//根据id查询
mongoTemplate.findById(1, User.class);

//查询所有
mongoTemplate.findAll(User.class);

//等值查询
mongoTemplate.find(Query.query(Criteria.where("name").is("小红")), User.class);

//<,>,>=,<=
mongoTemplate.find(Query.query(Criteria.where("age").lt(33)), User.class);

//and查询
mongoTemplate.find(Query.query(
	Criteria.where("name").is("小号").and("age").is(11)), User.class);
	
//or查询
Criteria criteria = new Criteria();
criteria.orOperator(Criteria.where("name").is("小号"), Criteria.where("name").is("小红"));
mongoTemplate.find(Query.query(criteria), User.class);

//and or 查询
mongoTemplate.find(Query.query(
    Criteria.where("age").is(22).
    orOperator(Criteria.where("name").is("小红"), criteria.where("name").is("小wangb"))), User.class);

//排序
mongoTemplate.find(new Query().with(Sort.by(Sort.Order.desc("age"))), User.class);

//分页
mongoTemplate.find(new Query().with(Sort.by(Sort.Order.desc("age"))).skip(3).limit(2), User.class);

//总条数
mongoTemplate.count(new Query(), User.class);

//去重
mongoTemplate.findDistinct(new Query(), "age", User.class, int.class);
```



### 8:文档更新

```java
@Test
public void test08(){
    Query query=new Query();
    query.addCriteria(Criteria.where("userid").is("1003"));
    UpdateResult content = mongoTemplate.updateFirst(query, new Update().set("content", "555555555"), Comment.class);
    System.out.println("修改数量:"+content.getModifiedCount());
    System.out.println("匹配条数:"+content.getMatchedCount());
}
```



```java
//更新第一条
Query query = new Query(Criteria.where("age").is(44));
mongoTemplate.updateFirst(query, 
                          new Update().set("age", 33).set("name", "小王八"), User.class);
//更新批量
mongoTemplate.updateMulti(query, new Update().set("age", 11), User.class);

//更新插入
UpdateResult age = mongoTemplate.upsert(query, new Update().set("age", 44), User.class);
System.out.println(age.getModifiedCount());//修改条数
System.out.println(age.getMatchedCount());//匹配条数
System.out.println(age.getUpsertedId());//插入id
```



### 9:文档删除

```java
//条件删除
mongoTemplate.remove(new Query(Criteria.where("age").is(44)), User.class);

//删除所有
mongoTemplate.remove(new Query(), User.class);
```
