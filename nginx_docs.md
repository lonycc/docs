## 查看nginx主进程id

`ps -ef | grep "nginx: master process" | grep -v "grep" | awk -F ' ' '{print $2}'`

## 平滑重启nginx

`kill -HUP nginx` 或 `/usr/local/nginx/sbin/nginx -s reload`

## [nginx中文站点](http://www.nginx.cn/)

## nginx 配置文件

```
user www-data;
pid /var/run/nginx.pid;
worker_processes auto;  #定义了nginx对外提供web服务时的worker进程数, 不能确定时设置为auto, 也可将其设为CPU内核数
worker_rlimit_nofile 1000;  #更改worker进程的最大打开文件数限制(ulimit -a), 如果出现"too many open files"错误, 尝试把这个值改大

events {
    use epoll;  #设置用于复用客户端线程的轮询方法
    multi_accept on;  #告诉nginx收到一个新连接通知后接受尽可能多的连接
    worker_connections 1024;  #设置可由一个worker进程同时打开的最大连接数。如果设置了上面提到的worker_rlimit_nofile，我们可以将这个值设得很高。记住，最大客户数也由系统的可用socket连接数限制（ulimit -n），所以设置不切实际的高没什么好处。
}

http {
    
    server_tokens  off;  #隐藏nginx版本号
    sendfile on;     #可以让sendfile()发挥作用
    tcp_nopush on;   #告诉nginx在一个数据包里发送所有头文件, 而不是一个接一个的发送
    tcp_nodelay on;  #不缓存数据, 而是一段一段的发送. 当需要及时发送数据时, 就应该给应用设置这个属性
    
    server  {
        listen 80 default;   
        rewrite ^(.*) http://www.domain.com permanent;   #流量收集并导入
        return 500;    #禁止显示任何有效内容
        
        location / {
        
        }
        location ~ \.php {
            
        }
   }
}
```
       
`proxy_pass http://bbs_server_pool;`  #用于指定反向代理的服务器池。

`proxy_set_header Host $host;`  #当后端Web服务器上也配置有多个虚拟主机时，需要用该Header来区分反向代理哪个主机名。

`proxy_set_header X-Real-IP $remote_addr;` #如果后端Web服务器上的程序需要获取用户IP，请从该Header头获取。

`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;` # Nginx虚拟主机配置 每台虚拟主机都可以是一个独立的网站，可以具有独立的域名。虚拟主机提供了在同一台服务器/同一组Nginx进程上运行多个网站的功能。



## location 匹配命令

`~`   #区分大小写,匹配正则表达式

`~*`  #不区分大小写,匹配正则表达式

`^~`  #普通字符匹配，如果该选项匹配，只匹配该选项，不匹配其他选项；一般用于匹配目录

`=`   #普通字符精确匹配

`@`  #定义一个命名的location，使用在内部定向时，例如error_page，try_files

`!~`  #区分大小写不匹配
 
 
### 例子

`location = / {}`  #只匹配/

`location / {}`   #匹配所有

`location ^~ /images/ {}` #匹配任何以/images/开始的请求，并停止匹配其它location

`location ~* \.(gif|jpg|jpeg)$ { }`  #匹配后缀名为gif/jpg/jpeg的文件

`error_page 404 = @fetch;`

`location @fetch( )` 

`location ~* /.bat { deny all; }` #禁止访问.bat文件

`location ^~ /configs/ { deny all; }`  #禁止访问configs目录

```
server{ 
    listen 80 default_server;  #禁止ip直接访问
    server_name _;
    return 404;
    rewrite ^ http://www.domain.com$request_uri?;
}
```

## rewrite语法
 
### 1、标志符

`last`  #完成rewrite

`break`  #中止rewrite, 不再继续匹配

`redirect` #返回临时重定向HTTP状态302

`permanent` #返回永久重定向HTTP状态301
 
### 2、判断表达式

`-f` 和`!-f`  是否存在文件
`-d`和`!-d`  是否存在目录
`-e`和`!-e`  是否存在文件或目录
`-x`和`!-x`  文件是否可执行
 
<br/>

## 全局变量

`$host`  #主机名

`$server_port`  #端口号

`$request_uri`  #请求的uri

`$document_root`  #应用根目录

`$request_filename`  #请求的文件
 
`.*`任意长度的任意字符, 任意值

`$1` 引用`.*`

`rewrite ^(.*)$ /index.php?s=$1 last;`  #在浏览器输入test.com/aaa.jpg, 自动跳转为test.com/index.php?s=aaa.jpg

`rewrite ^/yourdomain/(.*)$ /yourdomain/index.php?s=$1;`  #二级目录

`rewrite (/.*)$ /temp/index_txt.html`;   #自动跳转到xxx.com/temp/index_txt.html


## 负载均衡配置

```
upstream allserver {
    ip_hash;     #将某个ip的请求定向到同一台后端，建立稳固session
    server 127.0.0.1:8083 down;  #表示当前的server暂时不参与负载；
    server 127.0.0.1:8084 weight=3;   #默认为1，weight越大，负载的权重也越大；
    server 127.0.0.1:8001;
    server 127.0.0.1:8002 backup;   #备用机器，仅当其他机器down或忙时负载
}
```

## 文件防盗链

> 编译安装nginx, 需要开启--with-http_secure_link_module --with-http_stub_status_module; 此外要配合程序来设置 

`secure_link md5_hash[,expiration_time]`

`secure_link_md5 secret_token_concatenated_with_protected_uri`

```
location / {
    secure_link $arg_st,$arg_e;
    secure_link_md5 ttlsa.com$uri$arg_e;
    
    if ($secure_link = "") {
        return 403;
    }
    if ($secure_link = "0") {
        return 403;
    }
}

<?php
$secret = 'ttlsa.com';  //密钥
$path = '/path/to/file.xxx';  //下载的文件
$expire = time() + 300;  //过期时间
$md5 = base64_encode(md5($secret . $path . $expire, true));
$md5 = strstr($md5, '+/', '-_');
$md5 = str_replace('=', '', $md5);
echo '<a href=http://s1.down.ttlsa.com/path/to/file.xxx?st='.$md5.'&e='.$expire.'>file.xxx</a>';
echo '<br>http://s1.down.ttlsa.com/path/to/file.xxx?st='.$md5.'&e='.$expire;

```

## 图片防盗链

`referer_hash_bucket_size 64;` #可放在server/location中

`referer_hash_max_size 2048;` #可放在server/location中

`valid_refererrs none | blocked | server_names | string ...;` #可放在server/location中, 指定合法来源

```
location ~* \.(gif|jpg|png|bmp)$ {
    valid_referers none blocked server_names *.domain.com ~\.google\. ~\.baidu\.; 
    if ($invalid_referer) {
        return 403;
        # rewrite ^/ http://www.example.com/403.jpg;
    }
}
```

## nginx启用http basic auth

```
server {
    listen 80;
    server_name xxx;
    auth_basic "test http basic auth";
    auth_basic_user_file /usr/local/nginx/conf/htpasswd.users;
    ...
}
```
`htpasswd.users`里存放里用户名和密码, 其生成方式为

`yum -y install httpd-tools`

`htpasswd -bc /usr/local/nginx/conf/htpasswd.users username passwd`

 
## nginx 反向代理google

```
proxy_cache_path /var/www/cache/ levels=1:2 keys_zone=one:100m max_size=1g;
proxy_cache_key $host$request_uri;

upstream google{
     server 74.125.200.103 max_fails=3 fail_timeout=10s;
     server 74.125.200.105 max_fails=3 fail_timeout=10s;
     server 74.125.200.99 max_fails=3 fail_timeout=10s;
     server 74.125.200.147 max_fails=3 fail_timeout=10s;
     server 74.125.200.104 max_fails=3 fail_timeout=10s;
}
 

server {
    listen 80;
    server_name google.kfd.me;
    # rewrite ^/(.*) https://google.kfd.me$1 permanent;
}
 

server {
    listen 443;
    server_name google.kfd.me;
    ssl on;
    ssl_certificate /etc/ssl/private/google_kfd_me.crt;
    ssl_certificate_key /etc/ssl/private/kfd_me.key;
 

    location / {
        proxy_pass http://google;
        proxy_cache one;
        proxy_cache_valid 200 302 2h;
        proxy_cache_valid 404 1h;
        proxy_buffering off;
        proxy_cookie_domain google.com google.kfd.me;
        proxy_redirect https://www.google.com/ /;
        proxy_set_header HOST 'www.google.com';
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Accept-Encoding "";
        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header Accept-Language "zh-CN";
        proxy_set_header Cookie "PREF=ID=047818f19f6de346:U=0f622f33dd8549d11:FF=25:LD=zh-CN:NW=1:TM=1325238577:LM=1332342444:GM=5:SG=1:S=rE01SyJh2w1IQ-Maw";
        sub_filter www.google.com google.kfd.me;
        sub_filter_once off;
    }
}
```



## 访问a站重定向到b站

```
server {
	server_name www.a.com
	rewrite ^/(.*)$ http://www.b.com/$1 permanent; 
}
```

## www和非www之间跳转

```
server {
	server_name a.com
	rewrite ^/(.*)$ http://www.a.com/$1 permanent; 
}
```

## 302跳转设置

```
server {
    listen 80;
    server_name downcc.com;
    rewrite ^/(.*)$ http://www.downcc.com/$1 redirect;
    access_log off;
}
```

`add_header X-Frame-Options DENY;`  #禁止被iframe

`add_header X-Frame-Options ALLOW-FORM http://xxx.com;` #只允许指定域名

`add_header X-Frame-Options SAMEORIGIN;`  #只允许同域名

`add_header Set-Cookie "HttpOnly=true";`

`add_header Access-Control-Allow-Origin www.domain.com;` #允许指定的域名跨域请求

`add_header Content-Security-Policy "default-src 'self';";` #允许所有资源, 但只能来自当前域名

<br/>

# 知乎的Content-Security-Policy配置

`default-src *; ` #所有资源, 任意来源

`img-src * data: blob:; ` #图片来源, URL/data/blob

`frame-src 'self' *.zhihu.com getpocket.com note.youdao.com read.amazon.cn; ` #在CSP2中该指令已被废除, 允许iframe的内容

`child-src 'self' *.zhihu.com; ` #CSP2中取代frame-src

`frame-ancestors: 'self' *.zhihu.com; ` #只允许zhihu.com顶级域名下的来源嵌套

`script-src 'self' *.zhihu.com unpkg.zhimg.com *.google-analytics.com res.wx.qq.com 'unsafe-eval'; ` #脚本文件来源

`style-src 'self' *.zhihu.com 'unsafe-inline'; ` #样式文件来源, 允许来自zhihu.com域名下的, 以及内联css

`connect-src * wss:;` #访问源控制, URL/wss

[CSP](https://content-security-policy.com/)
