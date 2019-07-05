## MySQL - MongoDB命令行工具及SQL语法对比 
> 关系数据库的一般由数据库（database）、表（table）、记录（record）三个层次概念组成，MongoDB是由数据库（database）、集合（collection）、文档对象（document）三个层次组成。MongoDB对于关系型数据库里的表，但是集合中没有列、行和关系概念。 

## windows cmd下进入mongo操作，先输入mongo

### DB shell操作

- show dbs; 查看数据库列表

- db.dropDatabase();  删除当前使用的数据库

- db.cloneDatabase("127.0.0.1");  从指定主机上克隆数据库到当前数据库

- db.copyDatabase("mydb","temp","127.0.0.1");  将本机的mydb数据库复制到temp数据库

- db.repairDatabase();  修复当前数据库

- db.getName();db;  查看当前数据库

- db.stats();  查看当前db状态

- db.getMongo();  查看当前db的连接机器地址

- db.version(); 查看db版本

- use db_name;  切换库

- db.getPrevError();  查询上次错误

- db.resetError();  清除错误记录

### 帮助操作

- help

- db.help();
- db.collName.help();
- db.collName.find().help();
- rs.help();

### 用户相关

- db.addUser("name");

- db.addUser("userName","pwd123",true);  添加用户，设置密码，只读

- db.auth("userName","123123");  数据库认证、安全模式

- show users;

- db.removeUser("userName");  删除用户

### Collection聚集集合操作

- show collections;  查看当前库中所有集合

- db.createCollection("collName",{autoIndexID:true,size:20,capped:true,max:100});  创建集合

capped 若为true，则启用封顶集合。封顶集合是固定大小的集合，会自动覆盖最早的条目。

autoIndexID 若为true，自动创建索引_id字段的默认值是false。

size 指定最大字节封顶集合

max 指定封顶集合允许在文件的最大数量

- db.getCollection("account");  获取指定名称的集合

- db.getCollectionNames();  获取当前db的所有集合

- db.collName.count();  集合中数据条数

- db.collName.dataSize();  查看数据空间大小

- db.collName.getDB();  得到当前集合所在的db

- db.collName.stats();  集合状态

- db.collName.totalSize();  集合总大小

- db.collName.storageSize();  集合存储空间

- db.collName.getShardVersion();  

- db.collName.renameCollection("users");  集合重命名

- db.collName.drop();  删除当前集合

- db.collName.find();  查询所有记录，默认每页20条，用it命令来迭代显示下一页

- db.collName.distinct("name");  过滤重复数据，相当于 select distinct name from collName;

- db.collName.find({age:22}); 相当于 select * from collName where age=22;

- db.collName.find({age:{$gt:22}}); 相当于 select * from collName where age>22;

$lt; 小于  $gte; 大于等于  $lte;  小于等于

- db.collName.find({age:{$gte:23,$lte 26})); 相当于select * from collName where age>=23 and age<=26;

- `db.collName.find({name:/^mongo/})`;  相当于select * from collName where name like 'mongo%';

- `db.collName.find({name: {$not: /mongo/} });`  not like

- db.collName.find({},{name:1,age:1});  相当于select name,age from collName;

- db.collName.find({age:{$gt:25}},{name:1,age:1});  相当于select name,age from collName where age>25;

- db.collName.find().sort({age:1});  按年龄升序排列

- db.collName.find().sort({age:-1});  按年龄降序排列

- db.collName.find({name:'zhs',age:22});  相当于select *from collName where name='zhs' and age=22;

- db.collName.find().limit(5);  查询前5条

- db.collName.find().skip(10);  查询10条以后数据

- db.collName.find().limit(10).skip(5);  查询5-10之间的数据，可用于分页，limit是pageSize,skip是第几页*pageSize

- db.collName.findOne();  查询第一条数据
- db.collName.find({age:{$gte:25}}).count();  相当于select count(*) from collName where age>=25;

- db.collName.find({sex:{$exists:true}}).count(); 相当于select count(sex) from collName;

### 索引

- db.collName.ensureIndex({name:1});   为name字段创建索引

- db.collName.ensureIndex({name:1,ts:-1}); 

- db.collName.getIndexes();  查询当前集合所有索引

- db.collName.totalIndexSize();  查询当前集合索引大小

- db.collName.reIndex();  查询当前集合所有index信息

- db.collName.dropIndexes(); 删除所有索引

- db.collName.dropIndex("index_name");  删除指定索引

- db.collName.save({name:'haha",age:25,sex:true});  添加数据

- db.collName.insert({_id:1,"name":"n2"});  若主键为1的记录存在，则会报错，而save则会修改记录

- db.collName.update({age:25},{$set:{name:'changeName'}},false,true);  相当于update collName set name='changeName' where age=25;

- db.collName.update({name:'Lishi'},{$inc:{age:50}},false,true);相当于update collName set age=age+50 where name='Lishi';

- db.collName.update({name:'Lishi'},{$inc:{age:50},$set:{name:'hoho'}},false,true};  相当于 update set age=age+50,name='hoho' where name='Lishi';

- db.collName.remove({age:132});  相当于 delete from collName where age=132;

### 查询修改删除

> db.collName.findAndModify({  
... query: {age: {$gte: 25}},   
... sort: {age: -1},   
... update: {$set: {name: 'a2'}, $inc: {age: 2}},  
... remove: true  
... });  

   
> db.runCommand({ findandmodify : "collName",   
... query: {age: {$gte: 25}},   
... sort: {age: -1},   
... update: {$set: {name: 'a2'}, $inc: {age: 2}},  
... remove: true  
... });




### 简单的函数操作

- print("haha");

- tojson(new Object());   将一个对象转换成json

- tojson(new Object('a'));  

#### 1、循环添加数据

> for (var i = 0; i < 30; i++) {  
      db.users.save({name: "u_" + i, age: 22 + i, sex: i % 2});  
  };

这样就循环添加了30条数据，同样也可以省略括号的写法

> for (var i = 0; i < 30; i++) db.users.save({name: "u_" + i, age: 22 + i, sex: i % 2});  

也是可以的，当你用db.users.find()查询的时候，显示多条数据而无法一页显示的情况下，可以用it查看下一页的信息；

### 2、find 游标查询

> var cursor = db.users.find();  
  while (cursor.hasNext()) {   
    printjson(cursor.next());   
  }

这样就查询所有的users信息，同样可以这样写

> var cursor = db.users.find();  
  while (cursor.hasNext()) { printjson(cursor.next); }  

同样可以省略{}号

### 3、forEach迭代循环

>db.users.find().forEach(printjson);  
forEach中必须传递一个函数来处理每条迭代的数据信息

### 4、将find游标当数组处理

> var cursor = db.users.find();  

> cursor[4];  

取得下标索引为4的那条数据

既然可以当做数组处理，那么就可以获得它的长度：cursor.length();或者cursor.count();

那样我们也可以用循环显示数据

> for (var i = 0, len = c.length(); i < len; i++) printjson(c[i]);  

### 5、将find游标转换成数组

> var arr = db.users.find().toArray();  

> printjson(arr[2]);  

用toArray方法将其转换为数组

### 6、定制我们自己的查询结果

只显示age <= 28的并且只显示age这列数据

> db.users.find({age: {$lte: 28}}, {age: 1}).forEach(printjson);  

> db.users.find({age: {$lte: 28}}, {age: true}).forEach(printjson);  

排除age的列

> db.users.find({age: {$lte: 28}}, {age: false}).forEach(printjson);  

### 7、forEach传递函数显示信息

> db.things.find({x:4}).forEach(function(x) {print(tojson(x));});  

上面介绍过forEach需要传递一个函数，函数会接受一个参数，就是当前循环的对象，然后在函数体重处理传入的参数信息。


# 查询实例
```
db.jandan_duan.find()  #查看全部, 带.pretty() 美化显示效果
db.jandan_duan.count()  #统计数量
db.jandan_duan.find({'author':'光消失的地方'})  #
db.jandan_duan.find({'author':'光消失的地方', 'oo': {$gt, 500}})  #

db.jandan_duan.find().limit(10)  #取10条

db.jandan_duan.find().limit(10).skip(10) #相当于limit 10 offset 10

db.jandan_duan.find().limit(2).sort({'oo': -1},{'author': 1}).pretty()  #多个维度排序

db.jandan_duan.find().sort({'oo': -1}).limit(2) # limit可放后面

db.jandan_duan.find({'author': '光消失的地方'}, {'oo': 1,'xx': 1}).pretty()  # 返回指定field

db.jandan_duan.find({'$or': [{'oo': {'$gt': 1000}}, {'xx': {'$gt': 500}}]}).pretty()  #或条件

db.jandan_duan.find({'oo': {$gt: 0, $lt: 10}})  # 0<oo<10

db.jandan_duan.find({'author': null}).pretty()  # 匹配null

db.jandan_duan.find({'oo': {'$in': [100, 200, 300]}}).pretty()  # in/not in  ($in, $nin)


db.jandan_duan.find({'content': /日本/}).pretty()   # like  '%日本%'
db.jandan_duan.find({'content': /^日本/}).pretty()   # like '日本%'
db.jandan_duan.find({'content': /日本$/}).pretty()   # like '%日本'

db.jandan_duan.distinct('author')  # 唯一性

db.jandan_duan.aggregate([{$group: {}}])

db.news.find({'category':'car'}, {'title':1, 'pdate':1}) #只返回_id/title/pdate字段

db.news.find({'category':'car'}, {'content':0})  #只过滤content字段
```

<br/>

### 字段从String转Int
```
db.jandan_duan.find({'xx': {'$type': 2}}).forEach(function(obj) { 
    obj.xx = new NumberInt(obj.xx);
    db.jandan_duan.save(obj);
});

db.jandan_duan.find({'oo': {'$type': 2}}).forEach(function(obj) { 
    obj.oo = new NumberInt(obj.oo);
    db.jandan_duan.save(obj);
});
```
<br/>
### 字段从Int转String
```
db.jandan_duan.find({'oo': {'$type': 18}}).forEach(function(obj) { 
    obj.oo = new String(obj.oo);
    db.jandan_duan.save(obj);
});
```
<br/>

# mongodb 用户与权限

## 添加超级管理员

`use admin`

`db.createUser({user:'admin', pwd: '123456', roles: [{role: 'root', db: 'admin'}]})`

```
Built-In Roles（内置角色）：
1. 数据库用户角色：read、readWrite;
2. 数据库管理角色：dbAdmin、dbOwner、userAdmin；
3. 集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
4. 备份恢复角色：backup、restore；
5. 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
6. 超级用户角色：root  
// 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
7. 内部角色：__system
具体角色：
read：允许用户读取指定数据库
readWrite：允许用户读写指定数据库
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
root：只在admin数据库中可用。超级账号，超级权限
```

## 开启权限验证

`vim /usr/local/etc/mongodb.conf`

```
security:
    authorization: enabled
```

`mongod.restart`

> 注意: 如果是windows机器, 在配置文件中添加 auth=true

## 添加普通用户

`use mydb`

`db.createUser({user:'test', pwd: '123456', roles: [{role: 'readWrite', db: 'mydb'}]})`

## 用户验证

`use mydb`

`db.auth('username', 'password')`

## 备份数据库

`./bin/mongodump --host=127.0.0.1 --port=27017 --username=username -password=password --db=mydb --collection=mycollection -o /home/backup/`

## 还原数据库

`./bin/mongorestore --host=127.0.0.1 --port=27017 -uusername -ppassword --db=mydb --collection=mycollection --drop --dir=/home/backup/mydb/`

> 恢复时, 若加了--drop, 会先删除当前数据再恢复备份数据, 慎用

## 导出工具

`mongoexport --host=127.0.0.1 --port=27017 -uusername -ppassword --db=mydb --collection=mycollection --type=json -o /home/mydb/mycollection.json --fields "f1,f2..."`

## 导入工具

`mongoimport --host=127.0.0.1 --port=27017 -uusername -ppassword --db=mydb --collection=mycollection --type=json --file=/home/mydb/mycollection.json`

<br/><br/>


**pymongo**

```
import pymongo
from bson.objectid import ObjectId

# ObjectId(_id)  # 将string类型_id转为bson.objectid

m_client = pymongo.MongoClient('mongodb://localhost:27017')
m_client.list_database_names()
m_db = m_client.mydb  # or m_client['mydb']
m_db.auth('username', 'password')  # when needed
m_db.list_collection_names()
m_col = m_db.mycol  # or m_db['mycol']
m_col.find({'title': {'$not': re.compile('mongo')}})  # 和原生语句的区别在于正则语句的配置
m_col.find_one({'name': 'tony'}).sort('pdate')  #条件可为空
m_col.count_documents({})  # 统计
m_col.create_index([('uid', pymongo.ASCENDING)], unique=True)  # 创建索引
m_col.index_information()  # 索引信息
m_col.remove_one({})
m_col.remove_many({})
m_col.update_one({'_id': _id}, {'$set': {'key': 'value'}})
m_col.update_many({}, {})

rs = m_col.insert_many([dict(), dict(), dict()])  # 多条插入
rs.inserted_ids  # 返回多个_id
rs = m_col.insert_one(dict())  #单条
rs.inserted_id  # 单个_id


```
