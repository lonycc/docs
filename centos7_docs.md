## CentOS7的配置
<br>
### 1.语言编码

`locatle`  #查看系统支持哪些语言

`vi /etc/locale.conf`  #修改系统语言

或者

`localectl set-locale LANG=en_US.UTF-8`


### 2.配置网络

`cd /etc/sysconfig/network-scripts`

`ifcfg-eno1` #对应第一个网卡

`ifcfg-eno2` #对应第二个网卡

`ifcfg-io`   #对应本地回环

`systemctl restart network`  #重启网络服务


### 3.配置ssh

`vi /etc/ssh/sshd_config`

`PermitRootLogin no` #禁止root登入

`Port 19735`  #非常规端口

`systemctl restart sshd.service` #重启sshd.service


### 4.yum配置

`/etc/yum.conf`  #全局配置
```
[main]
#yum 缓存的目录，yum 在此存储下载的rpm 包和数据库，默认设置为/var/cache/yum
cachedir=/var/cache/yum/$basearch/$releasever

#安装完成后是否保留软件包，0为不保留（默认为0），1为保留
keepcache=0

#Debug 信息输出等级，范围为0-10，缺省为2
debuglevel=2

#yum 日志文件位置。用户可以到/var/log/yum.log 文件去查询过去所做的更新。
logfile=/var/log/yum.log

#有1和0两个选项，设置为1，则yum 只会安装和系统架构匹配的软件包
exactarch=1

#允许更新陈旧的RPM包
obsoletes=1

#是否启用插件，默认1为允许，0表示不允许。
plugins=1

#允许保留多少个内核包
installonly_limit=5

#bug管理
bugtracker_url=

#指定一个软件包，yum会根据这个包判断你的发行版本，默认是redhat-release，也可以是安装的任何针对自己发行版的rpm包。
distroverpkg=centos-release
```
`/etc/yum.repos.d/*.repo`  #yum仓库配置
```
#[REPO_ID] 用于区别各个不同的repository,唯一性
[base]
#name 是对repository的描述，支持像$releasever $basearch这样的变量;
name=CentOS-$releasever - Base name
#mirrorlist指定一个镜像服务器的地址列表，将$releasever和$basearch替换成自己对应的版本和架构，
#例如10和i386，在浏览器中打开，我们就能看到一长串镜可用的镜像服务器地址列表。
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=
#这个选项表示这个repo中定义的源是启用的，0为禁用
enabled = 1
#启用gpg的校验，确定rpm包的来源安全和完整性 0为禁止
gpgcheck=1
#定义用于校验的gpg密钥
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
#cost开销，默认是1000，开销越大，优先使用级越低。
#cost=
```

**配置阿里云的仓库**

`wget -O /etc/yum.repos.d/aliyun.repo http://mirrors.aliyun.com/repo/Centos-7.repo`

**使用光盘作为本地库**
```
1、挂载光盘至某目录
mount -r /dev/cdrom /mnt/cdrom
2、定义仓库
vim CentOS-local-ISO.repo
[centos7-ISO]
name=centos-local-iso
baseurl=file:///mnt/cdrom
enabled=1
gpgcheck=0
cost=100
3、查看可用仓库
yum repolist enabled
```

`创建本地仓库`
```
1、安装createrepo工具
yum install createrepo
2、建立仓库资源，建立header文件
createrepo packages/
ls packages/
3、定义本地仓库local.repo
[localrepo]
name=local-repo
baseurl=file:///root/packages
gpgcheck=0
enabled=1
```

`其他一些源`
```
# 安装该源
rpm -ivh http://mirror.city-fan.org/ftp/contrib/yum-repo/city-fan.org-release-1-13.rhel6.noarch.rpm
# 升级libcurl
yum upgrade libcurl
# 卸载该源
rpm -e city-fan.org-release

# epel源
yum install epel-release -y
```


`yum常用命令`
```
yum repolist enabled #查看可用的源
#对于安装类选项,可以加-y来跳过确认步骤
yum [options] [command] [package...]
yum install pkg1 [...]  #安装指定包
yum remove | erase pkg1 [...]  #卸载指定包
yum autoremove [pkg1] [...]  #同时卸载相关依赖包
yum update [pkg1] [...] #升级指定包
yum check-update [pkg1] #检测已安装包的更新信息
yum reinstall pkg1 [...] #重新安装指定包
yum downgrade pkg1 [...] #降级安装指定包
yum info [...]  #查询[指定]包信息
yum info updates #查询可更新的rpm包信息
yum info installed  #查询已安装rpm包信息
yum install extras #查询已经安装但不包含在资源库中的rpm包信息
yum list #列出资源库中所有rpm包
yum list [updates|installed|extras]
yum search str1 [...] #搜索匹配特定字符的rpm包
yum provides php  #搜索有包含特定文件名的rpm包
yum clean [pkg|headers|metadata|expire-cache|rpmdb|plugins|all] #清理缓存
yum makecache #创建缓存
yum grouplist #包组列表
yum group info ""  #包组信息
yum group install "" #安装包组
yum group remove "" #卸载包组
yum repolist [all|enabled|disabled] #列出全部/可用/不可用仓库
yum repoinfo [all|enabled|disabled] #列出信息
```
### 5. 一些系统命令
`cat /etc/redhat-release`  #查看系统版本

`yum groupinstall "Development Tools"` #安装开发工具

### 6. `/etc/sudoers`

权限是440，修改之前，先更改权限为640，修改之后要重新设为440
```
## 允许root运行所有命令
root    ALL=(ALL)       ALL

## 允许wheel用户组中的所有用户运行所有命令
%wheel  ALL=(ALL)       ALL

## 允许wheel用户组中的所有用户不用输入密码运行命令
%wheel        ALL=(ALL)       NOPASSWD: ALL
```
### 7. 后台进程管理工具supervisor
```
# 先下载ez_setup.py并安装easy_install
wget --no-check-certificate https://bootstrap.pypa.io/ez_setup.py -O - | sudo python

# 安装supervisor
easy_install supervisor

# yum方式安装
yum install epel-release
yum install -y supervisor

# 查看相关执行程序
ll /usr/bin | grep supervisor
# supervisord  守护进程服务, 用于接收进程管理命令
# supervisorctl 客户端, 用户和守护进程通信, 发送管理进程的指令
# echo_supervisord_conf 生成初始配置文件程序

# 守护进程配置
mkdir /etc/supervisor
echo_supervisord_conf > /etc/supervisor/supervisord.conf

# 管理进程配置
mkdir -p /etc/supervisor/config.d
# 在supervisord.conf里[include]配置引入

# tomcat.ini
[program:tomcat]
command=/opt/apache-tomcat-8.0.35/bin/catalina.sh run
stdout_logfile=/opt/apache-tomcat-8.0.35/logs/catalina.out
autostart=true
autorestart=true
startsecs=5
priority=1
stopasgroup=true
killasgroup=true

# 启动进程
supervisord
supervisorctl #进入进程管理交互
supervisorctl status | update | reread
supervisorctl stop | start | restart tomcat

# 开机启动supervisor服务
cd /lib/systemd/system
vi supervisor.service

## 设置开机启动
systemctl enable supervisor.service
systemctl daemon-reload
chmod 766 supervisor.service

# 创建supervisor启动脚本
/etc/rc.d/init.d/supervisor
chmod 755 /etc/rc.d/init.d/supervisor
chkconfig supervisor on
```

### 8. firewalld
```shell
#安装服务
sudo yum install firewalld

#查看服务状态
systemctl status firewalld.service
firewall-cmd --state
firewall-cmd --reload #平滑重启
firewall-cmd --complete-reload #重启
firewall-cmd --get-zones  #查看全部区域
firewall-cmd --get-default-zone #查看默认区域
firewall-cmd --get-active-zones #查看活跃区域
firewall-cmd --get-zone-of-interface=eth0 #查看接口所属区域
firewall-cmd --set-default-zone=public #设置默认区域
firewall-cmd [--zone=public] --list-services #查看当前开放服务
firewall-cmd [--zone=public] --list-ports #查看当前开放端口
firewall-cmd [--zone=public] --get-services #查看还有哪些服务，在列表中的服务是放行的
firewall-cmd [--zone=public] --query-service ftp #查看ftp服务是否支持，返回yes或者no
firewall-cmd [--zone=public] --add-interface=eth0 #开放接口
firewall-cmd [--zone=public] --add-service=http  #临时开放
firewall-cmd [--zone=public] --permanent --add-service=http #永久开放http服务
firewall-cmd [--zone=public] --add-port=80/tcp --permanent  #永久添加80端口
firewall-cmd [--zone=public] remove-service=http --permanent #移除服务

firewall-cmd --panic-on    #拒绝所有包
firewall-cmd --panic-off   #取消拒绝状态
firewall-cmd --query-panic #查看是否拒绝

#要添加的端口没有服务对应, 需新建服务/usr/lib/firewalld/services目录下
#例如要添加phpfpm的9000端口, 添加后要重启firewalld.service
vi /usr/lib/firewalld/services/phpfpm.xml
<?xml version="1.0" encoding="utf-8"?>

<service>
<short>php fpm</short>
<description>php process manager</description>
<port protocol="tcp" port="9000"/>
</service>
```

### 9. iptables

> centos 7 默认使用firewalld, 若要使用iptables，则先关闭firewalld，再设置iptables

```
systemctl stop firewalld.service
systemctl disable firewalld.service
yum -y install iptables-services
vi /etc/sysconfig/iptables

systemctl restart iptables.service
systemctl enable iptables.service
```

### 10. systemctl

> 系统 管理 守护进程、工具和库的集合。 一般而言，systemd是其它守护进程的父进程。

`systemctl --version`  #查看版本

`systemd-analyze`  #分析systemd启动进程

`systemd-analyze blame`	 #分析启动时各个进程花费时间

`systemd-analyze critical-chain` #分析启动时的关键链

`systemctl list-unit-files`	#列出所有可用单元

`systemctl list-units`	#列出所有运行中单元

`systemctl --failed` #列出所有失败单元

`systemctl is-enabled crond.service` #检查某个单元是否启用

`systemctl status firewalld.service` #

`systemctl list-unit-files --type=service` #

`systemctl start|restart|stop|reload|status|kill httpd.service` #管理服务

`systemctl is-active|enable|disabled httpd.service` #检查状态|启用|禁用服务

<br/>

### 11. 升级内核

`uname -r` # 先查看内核版本

`rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org`  # 导入public key

`rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm`  # 安装ELRepo

`yum --enablerepo=elrepo-kernel install kernel-lt -y` # 安装内核

`vi /etc/default/grub`  # 编辑grub配置文件, 将`GRUB_DEFAULT=saved` 改成 `GRUB_DEFAULT=0`

`grub2-mkconfig -o /etc/grub2-efi.cfg` # 配置生效

`reboot`  # 重启

`uanme -r`  # 查看内核版本

### 12. 时间同步

> centos 7版本使用chrony全面替换ntpdate，故我们直接安装chrony

`yum install -y chrony`

更换时区设置

`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone`

修改配置文件加入南方网时间服务器

`vi /etc/chrony.conf`

`server time.domain.com iburst`

启动服务

`systemctl enable chronyd.service && systemctl start chronyd.service`

### 13. MongoDB服务

先配置源`vim /etc/yum.repos.d/mongodb.repo`

```
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

执行安装 `yum -y install mongodb-org`

验证安装 `rpm -qa | grep mongo` 和 `rpm -ql mongodb-org-server`
