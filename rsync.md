# rsync配置

## 服务端配置(192.168.1.10)

### 开启iptables的873端口

`-A INPUT -p tcp -m tcp --dport 873 -j ACCEPT`

### 安装rsync和xinetd服务

`yum install rsync xinetd`

### 修改rsync配置

`vi /etc/xinetd.d/rsync`

>将disbled = yes修改为disabled = no

### 配置/etc/rsyncd.conf

```
timeout = 300 # 超时时间
max connections = 10  #最大连接数, 0为不限制
log file = /var/log/rsyncd.log
log format = %t %a %m %f %b
[demo]  #模块名
path = /opt/web
exclude = api/ html/   #排除目录, 多个之间用空格隔开
comment = demo  #这个名字务必和模块名一致
uid = root   #运行rsync守护进程的用户
gid = root   #运行rsync守护进程的组
use chroot = no
port=873     
read only = yes
auth users = username #若有多个用户,则用逗号分隔
secrets file = /etc/rsync.pass
hosts allow = 192.168.1.200/255.255.255.255 #只允许该ip连接,多个ip可用逗号分隔
#hosts deny = 
list = yes
```

### 配置密码文件,一行一个用户

`vi /etc/rsync.pass`

> username:password

`chmod 600 /etc/rsync.pass`

### 启动xinetd服务

`service start xinetd`
<br/><br/><br/>

## 客户端配置(192.168.1.200)

### 安装客户端

`yum install rsync`

### 编辑密码文件/home/rsync.pass

`vi /home/rsync.pass`

> password

`chmod 600 /home/rsync.pass`

### 启动服务

`rsync -vzrtopg --port=873 --progress --delete username@192.168.1.10::demo /opt/web --password-file=/home/rsync.pass`

`-v` #详细输出

`-q --quiet` #精简输出

`-a, --archive`  #归档模式, 递归传输, 保持文件属性

`-c, --checksum`  #打开文件校验

`-r, --recursive`  #对子目录递归处理

`-o, --owner`  #保持文件属主信息

`-g, --group`  #保持文件属组信息

`-l, --link`  #保持软链接

`-p, --perms` #保持文件权限

`-t, --times` #保持文件时间信息

`-z, --compress` #对备份的文件在传输时进行压缩处理

`--delete`  #删除那些DST中SRC没有的文件

`--progress` #显示传输过程


> 当然可以做成脚本以定时任务来运行.

<br/><br/><br/>
## 编译安装rsync客户端或服务端也可以

```
yum install gcc
tar -zxvf rsync-x.x.x.tar.gz
cd rsync-x.x.x
./configure --prefix=/usr/local/rsync
make && make install
```

> 若服务端采用了编译安装的方式,则无需安装xinetd,启动方式略有不同: `./rsync --daemon --config=/etc/rsyncd.conf`


`rsync -av --delete --exclude-from="/root/exclude.list" root@219.135.157.226:/data/site/staticize/{219,144,168} /opt/www/staticize/`  # --delete参数以源为准, 对目标进行多删少补的同步
