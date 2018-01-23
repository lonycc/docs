## 使用nat模式

`vi /etc/sysconfig/network-script/ifcfg-enp0s3`

```
TYPE=Ethernet
BOOTPROTO=dhcp
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=enp0s3
UUID=2c663cb8-58c3-42c7-88bd-57b1174f99a1
DEVICE=enp0s3
ONBOOT=yes
```

`service network restart`

## 设置-》网络-》高级-》端口转发

添加一条规则：

名称: `ssh`

协议：`tcp`

主机IP：为空

主机端口：`2222`

子系统IP：为空

子系统端口：`22`

## 然后可以用ssh客户端连接127.0.0.1的2222端口

[mac下virtualbox虚拟centos7网络配置](http://www.jianshu.com/p/e6ba699b5992)
