# [Redis](http://doc.redisfans.com/index.html)

> 一个内存Key-Value高速读写数据库, 支持数据持久化。支持的数据类型有String、Hash、List、Set和Zset。支持master-slave模式。

`config get *` #获取全部配置

`config set [name] [value]`  #修改配置

## Redis for Windows 安装

`redis-server --service-install redis.windows.conf --port 6379 --service-name Redis --loglevel verbose`  #安装服务

`redis-server --service-uninstall`  #卸载服务

`redis-server --service-start`  #启动服务

`redis-server --service-stop`  #停止服务

`redis-server redis.windows.conf`  #非安装时启动服务

`redis-cli shutdown` #停止服务

## Redis 数据类型

**String**

> key => value, 二进制安全. 一个key最大能存储512MB.

`set name tony` #设置

`get name` #获取

`del name` #删除

**Hash**

> 一个String类型的key和value的映射表, 适合存储对象. 每个hash可以存储 2^32 - 1 键值对.

`hmset hash_demo:1 username tony age 26 gender male`

`hgetall hash_demo:1`

**List**

> 字符串列表, 按插入顺序排序. 列表最多可存储 2^32 - 1 元素.

`lpush list_demo tony`  #左边push

`lpush list_demo andy`

`rpush list_demo su` #右边push

`lrange list_demo 0 10` #取出

**Set**

> Set是String类型的无序集合, 元素具有唯一性. 最大成员数为 2 ^ 32 - 1

`sadd set_demo v1` #添加

`sadd set_demo v2`

`sadd set_demo v1` #由于已经添加过, 会被忽略

`smembers set_demo`  #取出

**zset**

> 有序集合, 不允许重复成员. 每个元素会关联一个double类型的分数, 通过分数从小到大排序, 分数可重复.

`zadd zset_demo 0 v1`  #添加

`zadd zset_demo 0 v2`

`zadd zset_demo 0 v1`

`zrangebyscore zset_demo 0 1000`  #取出

<br/>

## Redis 命令

`del key` #删除一个key

`dump key`  #序列化key, 返回序列化后的值

`exists key` #是否存在key

`expire key [seconds]` #为key设定过期时间(秒)

`pexpire key [millseconds]`

`expireat key [timestamp]` #为key设定过期时间戳

`pexpireat key [ms_timestamp]` #为key设定过期时间戳, 以毫秒为单位

`keys [pattern]` #查找符合给定模式的key

`move key db` #将当前数据库的key移动到给定数据库db中

`persist key` #移除key的过期时间

`ttl key` #以秒为单位返回key的剩余过期时间

`pttl key` #以毫秒为单位返回key的剩余过期时间

`randomkey` #随机取回一个key

`rename key newkey` #修改key名称

`renamenx key newkey` #仅当key不存在时改名

`type key` #返回key的类型

`select 1` #切换到数据库1, 默认在数据库0


## Redis HyperLogLog

> Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。

> 基数是指数据集中不重复元素的个数。

`pfadd key element [element ...]` #添加元素到hyperloglog中

`pfcount key [key ...]` #返回给定hyperloglog的基数值

`pfmerge destkey sourcekey [sourcekey ...]` #将多个hyperloglog合并


## Redis 发布订阅

> Redis发布订阅(publish/subscribe)是一种消息通信模式：发送者发送消息, 订阅者接收消息.


`subscribe channel_1 [channel_2 ...]` #起一个客户端, 订阅若干个频道, 将会进入监听状态, 有消息就能收到

`pushlish channel_1 "what the fuck"` #另起一个客户端, 发布一条消息到channel_1

`unsubscribe channel_1 [channel_2 ...]` #取消订阅频道

`punsubscribe pattern [pattern ...]` #退订所有给定模式的频道

`pubsub [subcommand] [argument [argument ...]]` #查看订阅与发布系统状态

`psubscribe pattern [pattern ..]` #订阅多个给定模式的频道

## Redis 事务

> 事务是一个原子操作, 其中的命令要么全部被执行, 要么全部不被执行. 事务是一个单独的隔离操作, 其中所有命令都会被序列化、按顺序执行; 执行过程中不会被其他客户端发来的命令请求打断.

`multi` #开始事务

事务命令在中间

`exec` #执行事务

`discard` #取消事务

`watch key [key ..']` #监视多个事务, 若事务执行前这些key被改动, 事务将被打断.

`unwatch` #取消watch命令对所有key的监视.

## Redis 脚本

> Redis脚本使用Lua解释器来执行脚本.

`eval script numkeys key [key ...] arg [arg ...]` #执行lua脚本

`eval "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second`

`evalsha [sha_hash] numkeys key [key ...] arg [arg ...]` #根据给定sha1校验码, 执行缓存中脚本

`script exists script [script ...]` #查看指定脚本是否被保持在缓存中

`script flush` #从缓存中移除所有脚本

`script kill` #杀死当前运行的脚本

`script load [script]` #将脚本添加到缓存中, 但不立即执行

## Redis 连接

> 用于连接redis服务.
 
`auth [your_pass]` #验证密码

`echo [message]` #打印消息

`ping` #查看服务是否运行

`quit` #关闭连接

## Redis 安全

- 1. 绑定IP来控制

`vi redis.conf`

`bind 127.0.0.1`

- 2. 设置密码, 以提供远程登录

`vi redis.conf`

`requirepass your_strong_password`

`redis-cli -h [ip] -p [port]`

登录以后, 执行命令会提示权限不够, 则需要授权

`auth [your_strong_password]`

当然也可以在登录时就指定密码

`redis-cli -h [ip] -p [port] -a [your_strong_password]`

- 3. 临时设置密码

`config set requirepass your_strong_password`

`config get requirepass`

## Redis 基准测试

`redis-benchmark -h [host] -p [port] -n 1000`

`-c` #指定并发连接数, 默认50

`-d` #以字节形式指定SET/GET值的数据大小, 默认2

`-s` #指定服务器socket

`-k` #1=keep alive 0=reconnect, 默认1

`-n` #指定请求数, 默认10000

`-csv` #以csv格式输出

`-t` #仅运行以逗号分隔的测试命令列表


## Redis 服务器

> 管理redis服务.
 
`bgrewriteof` #异步执行一个aof文件重写操作

`bgsave` #后台异步保存数据库数据到磁盘

`client list` #客户端列表

`client getname` #获取连接名称

`client pause [timeout]` #在指定时间内(ms)停止运行命令

`client setname [connection-name]` #设置当前连接名称

`client kill [ip:port] [id client_id]` #关闭客户端连接

`cluster slots` #获取集群节点的映射数组

`info [server|clients|memory|cpu|stats|cluster]` #获取redis服务器各种信息和统计数值

`config rewrite` #将config set做的修改写到redis.conf里

`config resetstat` #重置info命令中的某些统计数据

`dbsize` #获取当前数据库中key的数量

`debug segfault` #执行一个非法内存访问, 让redis崩溃

`debug object [key]` #获取key的调试信息

`flushdb` #删除当前数据库所有key

`flushall` #删除所有数据库所有key 

`lastsave` #最近一个保存时间

`monitor` #实时打印redis服务器接收到的命令

`role` #返回主从实例所属角色

`save` #异步保存数据到硬盘

`shutdown [nosave|save]` #异步不保存/保存数据到硬盘, 并关闭服务器

`slaveof [host] [port]` #将当前服务器转变为指定服务器的slave
`slaveof no one` #将当前服务器转为master

`slowlog [subcommand] [argument]` #管理慢日志 
`slowlog get 2` #取2条  
`slowlog len` #数量  
`slowlog reset` #清空slowlog

`sync` #同步主服务器

## Redis 管道技术

> 管道技术可以在服务端未响应时, 客户端继续发送请求, 并最终一次性读取响应.

<br/>

## redis 命中率计算

`telnet localhost 6379` # 远程连接redis服务端

`info`  # 查看基本信息

```
keyspace_hits:7449297  #命中key的次数
keyspace_misses:101207 #没命中key的次数
used_memory:1178288 #分配的内存总量
expired_keys:97588  #运行以来过期的key
evicted_keys:0      #运行以来删除过的key
```

则命中率=`keyspace_hits / (keyspace_hits + keyspace_misses)`, 得到结果`98.66%`.

[redis-stat查看工具](https://github.com/junegunn/redis-stat)
