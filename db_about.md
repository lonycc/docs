## sqlite 重建表过程

`alter table table_name rename to tale_temp_name;`  # 修改表名为临时表

`create table table_name(a1 integer primary key not null autoincrement, a2 char(6) not null, a3 vchar(6), a4 int);` # 新建表

`insert into table_name(a1, a2, a3, a4) select a1,a2,a3,a4 from table_temp_name;`   # 从临时表导入数据到新表

`update sqlite_sequence set seq=1 where name='table_name'`; # 更新自增量

`drop table o_stream_dishi`;  # 删除临时表

## [sqlite 导出表结构和数据](http://blog.csdn.net/flyingroc0209/article/details/51721206)

> 先去[官网](https://www.sqlite.org/download.html)下载工具. 解压后进入命令行

### 导出sql

`sqlite3 test.db`

`.output out.sql`

`.dump`

### 从sql导入

`sqlite3 test.db`

`.read demo.sql`

<br/><br/>

## [淘宝数据库内核分析](http://mysql.taobao.org/monthly/)

## mysql 一些语句

`where > group by > having > order by > limit`  # sql关键词顺序

`CREATE TABLE new_table SELECT * FROM old_table WHERE 1=2;`  # 表结构复制

`create new_table like old_table;` # 复制表结构

`create table new_table as (select * from old_table);`  # 拷贝数据到新表, 但新表不会有主键或索引

`insert into new_table select * from old_table;`  # 表数据复制

`insert into new_table(field_1,field_2,...) select field_1,field2,... from old_table;`  # 表数据复制, 指定字段

`select * into table_2 from table_1 where 1=2;`  # 表结构复制

`select * into table_2 from table_1;`  # 表结构和数据都复制

`create_time timestamp not null default CURRENT_TIMESTAMP on update CURRENT_TIMESTAMP` # 创建timestamp类型字段

`update db1.table_name as a left join db2.table_name as b on a.username=b.username set a.password=b.password;`  # 批量更新

`select col_name, count(*) as c from table_name group by col_name order by c desc;` # 聚合排序

`update tbl_test set val = case id when 1 then 2 when 2 then 3 end where id in(1, 2);` # case when

`mysql -h{$hostname} -P{$port} -u{$username} -p{$password} {$dbname} -e "{$sql}"`  # shell

`insert into table_name(k, v) values(1, '1-1') on duplicate key update v=values(v);` # k为unique key

`match(col_1,col_2,...) against (expr[search_modifier])`  # 可执行全文检索的语句

`SELECT user, count(*) AS cnt, GROUP_CONCAT(logintime SEPARATOR '\n') AS logintimes,GROUP_CONCAT(title SEPARATOR '\n') as titles FROM userlog GROUP BY user ORDER BY cnt desc;`  # group_concat() 

```
# goup_concat 默认截断1024个字符
show variables like '%group_concat%';
set global group_concat_max_len=99999;
set session group_concat_max_len=99999;
```

`insert into mysql.user(Host,User,Password) values('localhost','jeecn',password('jeecn'));`

`flush privileges;`

`create database jeecnDB;`

`grant all on jeecnDB.* to jeecn@'localhost' identified by 'jeecn';`

`grant [select,insert,update,delete,create drop,index,alter,grant,references,reload,shutdown,process,file] on jeecnDB.* to jeecn@'localhost' identified by 'jeecn';`

`flush privileges;`

`revoke priv_type on *.* from xxx@yyy;`  # 撤销权限

`show grants for xxx@yyy;`   # 查看权限
 
`select * from table_name where ptime between '2015-03-12' and DATE_ADD('2016-03-15', interval 1 day);` # 包括两个边界值

`select * from table_name where ptime >= '2015-03-12' and ptime < '2016-03-15' + interval 1 day;` 

`select date(pdate) from table_name;`   # 返回日期部分

`select date_format(pdate, format) from table_name;`  # 格式化日期

`select curdate();` or `select current_date();` # 当前日期

`select current_time();`  # 当前时间

`select current_timestamp();`  # 当前日期时间

`select date_diff(date1, date2);`  #返回两个日期之间的天数

`select unix_timestamp();`  # 当前unix时间戳

`select from_unixtime(unix_timestamp);`  # unix时间戳转日期时间

`select date_format(date_sub('2015-12-25',interval 10 day),'%m%d') as t1;`  # 取前十天日期

`select date_format(date_add('2015-12-25',interval 10 day),'%m%d') as t2;`  # 取后十天日期

`SELECT AUTO_INCREMENT FROM information_schema.tables WHERE table_name="tableName";`  # 查看auto_increment

`ALTER TABLE tableName auto_increment=number;`  # 修改auto_increment

`select  col_name from tb_name limit n, m;`  # 选取从第n+1条开始的m条记录

`insert into tb2(列1,列2) select 列1,列2 from tb1;` # 从表tb1中把某些列插入tb2对应的列中。

`create view view_name as select col_name from tb_name where condition;`  # 创建视图

`create or replace view view-name as select col_name from tb_name where condition;` # 更新视图

`drop view view_name;` # 删除视图

`replace into table_name (col_1,col_2,...) values (val_1,val_2,...);` # 对于唯一索引的字段，若该字段不变，则为update；否则为insert

`replace into table_name default values;` # 插入一空行，这对于没有非空约束的表才有效

`alter table table_name add unique (col_name);` # 创建唯一索引

`alter table table_name add index index_name(col_name);` # 创建普通索引

`alter table table_name add fulltext (col_name);` # 全文索引

`alter table table_name add primary key (col_name);` #主键索引

`alter table table_name add index index_name (col_1,col_2,...);` # 创建多列索引

## myisam 和 innodb 引擎的区别

| 特性 | myisam | innodb |
|:--:|:--:|:--:|
|文件构成|.frm文件存储表定义;.myd数据文件;.myi索引文件|基于磁盘的资源,大小一般不超过2GB|
|事务处理|不支持事务|支持事务,外键|
|增删查改|适合大量select操作|适合大量insert/update;delete from table时不会重新建立表,而是一行一行删除;load table from master对innodb不起作用|
|auto_increment|为insert和update自动更新该列|自动增长计数器为该列赋值,被存储在主内存中|
|表行数|当select count(*) from table,直接读;带where时同innodb|不保存具体行数,需要扫描表|
|锁|表锁|行锁|

`show engine innodb status;`  #查看innode引擎状态

`show full processlist;`  #查看所有进程列表

## 数据库死锁处理

`select * from information_schema.innodb_trx;`  #查看当前事务

`kill trx_mysql_thread_id;`  #杀死进程id

`select * from information_schema.innodb_locks;`  #查看当前锁定事务

`select * from information_schema.innodb_lock_waits;`  #查看当前等待锁事务

`show OPEN TABLES where In_use > 0;`  #查看当前使用的表

`show processlist;`  #查看进程列表

`kill id;`  #杀死进程id

<br/><br/>

```
整数(分有符号signed和无符号unsigned)
int(size)  默认为11位，4字节,  -2^31到2^31-1
int(M) [unsigned] [zerofill]  不满M位补0，但占用字节仍为4, 超过M位则不受影响
smallint(size) 默认6位，2字节, -2^15到2^15
tinyint(size) 默认4位，1字节,  0-255
mediumint(size) 默认9位，3字节, -2^23到2^23-1
bigint(size) 默认20位，8字节,  -2^63 到2^63-1

浮点数（分有符号和无符号）
float(size, d) 4字节
double(size, d)8字节
decimal(size, d) size+2字节
size规定数字位数，d规定小数位数。
decimal(10,4); //表示最大支持10位(包括小数)，小数4位, 小数位不满的默认补0

位
bit(m) 1~8字节，m取值1~64，默认为1
bit_or(1<<2) 01右移2位，得到100，即4
bit_and(2|3)
```

## 字符串类型
```
char(M) 默认为1，M字节，M为0~255之间整数
varchar(M)  值的长度M+1字节，M为0~65535之间整数(其中多一个字节用于存放长度)
tinytext  0~255字节，值的长度+2字节
tinyblob 0~255字节，值的长度+1字节
text   0~65535字节，值的长度+2字节
blob  0~65535字节，值的长度+2字节
mediumtext 值的长度+3字节
mediumblob
longtext  值的长度+4字节
longblob
varbinary(M)  值的长度+1字节，允许0~M字节的变长字节字符串
binary(M)  M字节，允许0~M字节的定长字节字符串


日期数据类型
date(yyyy-mm-dd)  4字节，最小1000-01-01，最大9999-12-31
datetime(yyyy-mm-dd hh:mm:ss)  8字节
timestamp(yyyymmmddhhmmss)  4字节，Unix时间戳
time(hh:mm:ss)  3字节
year 1字节，最小1901，最大2155。

其他数据类型
set   与enum相似。set最多拥有64个列表项目，并可存放不止一个choice。
enum(value1, value2, etc)  枚举类型，最多65535个值。
```

## mysql运算符
```
算数运算符
+、-、*、/、div(整除)、%
mod(a, b)=a%b

比较运算符
>、<、<=、>=、between、not between、in、not in、
<>、!=
<=>严格比较两个null值是否相等
like 匹配
regexp 正则表达式匹配
isnull、is not null

逻辑运算符
and/&&、or/||、xor、not/!

位运算符
&、|、<<左移、>>右移
~位非、^位异或

优先级比较
1、=
2、or、xor
3、&&
4、not
5、between、case  when
6、=、<=>、>、<、is、like
7、|
8、&
9、<<、>>
10、-、+
```

## SQL模式通配符
```
%  替代一个或多个字符
_   替代一个字符
[charlist]     替代在字符列中的任何单一字符
[^charlist]  替代不在字符列中的任何单一字符
[!charlist]

regexp正则模式通配符
. 匹配任何单个字符
[a-z]匹配任何小写字母
[0-9]匹配任何数字
b* 匹配零个或多个在它前面的字符b
^b 匹配b开始
b$  匹配b结尾
b{n}  b重复n次
```

## SQL内建函数
```
select function(col_name) from tb_name;

avg(col_name) 返回某列平均值
count(col_name) 返回某列列数
count(*) 返回被选行数
max(col_name) 返回某列最大值
min(col_name) 返回某列最小值
sum(col_name) 返回某列总和

Scalar函数
面向单一值, 返回基于输入值的一个单一值.
```

## 字符串函数
```
concat(str1,str2,...)  连接多个字符串
concat_ws(',',str1,str2,...) 连接多个字符串，以','作为分隔符
length(str)  计算字符串长度
lower(str)  小写
upper(str)  大写
ltrim(str)  截去字符串左侧空格
rtrim(str)
trim(str)
substring(str, start, len) 从指定位置开始从字符串取指定长度的子串
instr(str, substr)  计算子串在母串中的位置
left(str, len)  从左侧开始取一定长度的子串
right(str, len)
replace(str, substr1, substr2) 在str中，用substr2替换substr1
ascii(c)  获得字符的ASCII码值
char(c)  由ASCII码值获得字符
soundex(str)  计算一个字符串的发音特征
lpad(str, len, padstr) 用padstr左填充str至长度为10
rpad(str, len, padstr)
repeat(str, count)重复输出count次字符串str
reverse(str)字符串颠倒
elt(N, str1, str2, str3...) 返回strN
field(str, str1, str2, str3...) 返回str在字符串集中的位置
find_in_set(str, strlist) 从集合中返回str的位置
insert(str, x, y, instr) 将字符串str从x位置开始的y个字符替换为instr
greatest(set) 计算集合中最大值
least(set) 计算集合中最小值
```

## 日期时间函数
```
now()  返回当前日期时间，别名sysdate(), current_timestamp()
curdate()  返回当前日期，别名current_date()
curtime()   返回当前时间，别名current_time()

date(date)  返回日期或日期/时间表达式的日期部分。

extract(unit from date)返回日期/时间的单独部分，比如年、月、日、时、分等。
参数可取值如下：
microsecond、second、minute、hour、day、week、month、quarter、year、day_second、year_month、hour_second、minute_second


date_add(date_str, interval expr type)  日期增减，别名ADDdate()
date_sub() SUBdate()  # 日期减
datediff(date1, date2)  返回date1-date2的差额
daytime(date)  计算一个日期是星期几
date_format(date, format)  返回日期的指定格式

%a  %W  星期名  
%w  周的天(0=周日，6=周六)
%U  周(00-53)周日为第一天
%u  周(00-53)周一为第一天
%V  周(00-53)周日为第一天,与%X使用
%X  年，4位
%v  周(00-53)周一为第一天,与%x使用
%x  年，4位
%Y 年，4位
%y  年，2位
%j  年的天(001-366)
%r  时间,12小时(hh:mm:ss AM或PM)
%p  AM或PM
%h  小时(00-12)
%H  小时(00-23)   
%I  小时(01-12)
%l  小时(1-12) 
%k  小时(1-23) 
%b %M 月名              
%c  月,数值
%m  月(00-12)            
%d  月的天(00-31)      
%D  月的天(带英文前缀)   
%e  月的天(0-31)  
%i  分钟(00-59)     
%S  %s秒(00-59)  
%f   微秒
```

## 数学运算函数
```mysql
abs()  求绝对值
power(expr, n) 求指数
sqrt()  求平方根
rand()  生成一个随机数
ceiling() 舍入到最大整数
floor()  舍入到最小整数
round(m,d)  四舍五入,m为原数,d为精度
round(m)  取整
sin();  cos();  asin(); acos();  tan();  atan();  atan2(x,y);
cot();  pi();  degrees();  radians();
sign() 求数的符号  mod(x,y)  x%y
ln() 求对数，以e为底
log10()
log(m,n)  m为底n的对数
```

## 其他函数
```mysql
cast(expr as type)    类型转换
convert(expr, type)   type可选binary、char、date、datetime、signed integer、time、unsigned integer
select hex(convert('fds' using utf8));  查看fds的utf8十六进制编码
select convert(0xE5BE88 using utf8);  查看十六进制编码0xE5BE88代表的字符

coalesce(expr, value1, value2, ...) 返回包括表达式在内的第一个非空表达式，若全为空，则返回null。
ifnull(expr, value)

if(expr1, expr2, expr3) 如果expr1为真, 则返回expr2, 否则返回expr3
nullif(expr1, expr2)     若等价, 返回null；否则返回expr1
if(@col_name=Num, 0, 1) 

conv(N, f_base, t_base) 将N从f_base进制转换成t_base进制
bin(N)、oct(N)、hex(N)
```

## 系统函数
```mysql
database()    返回当前库名
version()      返回版本号
user()    返回当前用户名
encode(str, pass_str)    用pass_str作为密钥加密str
decode(str, pass_str)    用pass_str作为密钥解密str
md5(str)   md5算法计算str摘要
sha1(str)   sha1算法计算str摘要
uuid()  通用唯一识别码
password() 用户密码
```

##存储过程
```
create procedure sp_name(IN p_in)
begin
  select p_in;
  set p_in=2;
end

call sp_name()  # 调用
drop procedure sp_name;  # 不能在一个sp中删除另一个sp
show procedure status; #
show create procedure sp_name;
```

## 存储函数
```
delimiter $$
create function myfunc(a double,b double) returns double;
begin
 declare var1 double;
  ...
 set var1 = sin(a)*cos(b);
 ...
return var1*var2;
end $$
delimiter ;

select myfunct(1.3, 2.4);
```

## 表分区
```
# range分区

create table employees(
   id int not null primary_key,
   fname varchar(30),
   lname varchar(30),
   job_code int not null,
   store_id int not null
) engine=myisam default charset=utf-8
partition by range (store_id) (
   partion p0 values less than(6),
   partion p1 values less than(11),
   partion p2 values less than(16),
   partion p3 values less than(21),
   partion p4 values less than 10000    小于10000的保存在p4分区
);

# 删除一个分区上旧数据时, 只需删除分区即可

alter table employees drop partition p0;

# list分区, 基于列值匹配一个离散值集合中的某个值来进行选择。

partition by list(expr)  expr为某列值
create table employees(
   id int not null primary_key,
   fname varchar(30),
   lname varchar(30),
   job_code int not null,
   store_id int not null
) engine=myisam default charset=utf-8
partition by range (store_id) (
   partion p0 values in(1,2,3,4,5),
   partion p1 values in(6,7,8,9,10),
   partion p2 values in(11,12,13),
   partion p3 values in(14,15,16)
);

# hash分区, 基于用户表达式的返回值来选择的分区

partition by [linear] hash(store_id)
partitions 4;

# key分区

partition by linear key(id)
partitions 3;
```
