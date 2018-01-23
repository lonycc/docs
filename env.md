#  1. window 7 环境变量
```
android_home: D:\AndroidSDK
java_home: D:\Jdk
jre_home: D:\Jre
maven_home: D:\apache-maven-3.3.9
mongo_home: D:\Mongo
mysql_home: D:\mysql
node_home: D:\node
pgsql_home: D:\PostgresQL
php_home: D:\php56
python_home: D:\Python35
go_home: D:\go
GOPATH: F:\myjob\go

path:
%android_home%;%android_home%\tools;%android_home%\build-tools;%android_home%\platform-tools;%java_home%\bin;%jre_home%\bin;%maven_home%\bin;%mongo_home%\bin;%mysql_home%\bin;%node_home%;%node_home%\node_global;%php_home%;%pgsql_home%\bin;%python_home%;%python_home%\Scripts;

classpath:
.;%java_home%\lib\dt.jar;%jre_home%\lib\tools.jar;
```

<br/>

# 2. 安装配置

## 1. node.js

Windows系统下全局配置文件`C:\User\[用户名]\.npmrc`，升级node直接覆盖安装。

```
registry=http://registry.npm.taobao.org/
disturl=https://npm.taobao.org/dist
prefix=D:\nodejs\node_global
cache=D:\nodejs\node_cache
proxy=null
```


Mac系统下全局配置文件在`/usr/local/etc/npmrc`，升级node版本用`n stable`，但事先要全局安装`npm install -g n`。

## 2. git客户端

Windows系统下全局配置文件在`C:\User\[用户名]\.gitconfig`，Mac或Linux系统下在`~/.gitconfig`(root用户)

## 3. pip源

Windows系统配置文件`C:\User\tony\pip\pip.ini`，Mac和Linux配置文件`$HOME/.pip/pip.conf`
```
[global]
index-url=http://pypi.doubanio.com/simple/
timeout=60
find-links=http://download.example.com
trusted-host = pypi.doubanio.com
disable-pip-version-check=true 

[install]
ignore-installed=true
no-dependencies=yes
find-links=
	http://mirror1.example.com
	http://mirror2.example.com

[freeze]
timeout=10
```
**pip也允许设置环境变量**

`export PIP_DEFAULT_TIMEOUT=60` 效果同  `pip --default-timeout=60 [...]`

`export PIP_FIND_LINKS="http://mirror1.example.com http://mirror2.example.com"`

效果同

 `pip install --find-links=http://mirror1.example.com --find-links=http://mirror2.example.com`

当然也可以直接修改pip模块源码：

- Windows系统下

`%python_home%\Lib\site-packages\pip\cmdoptions.py`
```
index_url = partial(
    Option,
    '-i', '--index-url', '--pypi-url',
    dest='index_url',
    metavar='URL',
    #default=PyPI.simple_url,
    default=http://pypi.doubanio.com/simple/,
    help='Base URL of Python Package Index (default %default).')
```
`%python_home%\Lib\site-packages\pip\commands\search.py`
```
    def __init__(self, *args, **kw):
        super(SearchCommand, self).__init__(*args, **kw)
        self.cmd_opts.add_option(
            '--index',
            dest='index',
            metavar='URL',
            #default=PyPI.pypi_url,
            default=http://pypi.doubanio.com/simple/,
            help='Base URL of Python Package Index (default %default)')

        self.parser.insert_option_group(0, self.cmd_opts)
```

- Linux系统下

`easy_install -i http://pypi.doubanio.com/simple/ fabric`

`pip -i http://pypi.doubanio.com/simple/ install fabric`

或者直接修改配置文件

`easy_install`的配置文件`~/.pydistutils.cfg`

```
[easy_install]
index_url=http://pypi.tuna.tsinghua.edu.cn/simple
```

如果pip直接安装失败, 则可以去 `http://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml` 下载whl包, `pip install wheel`,  `pip install xxx.whl` 

<br/>

`pip install xxx -i http://pypi.douban.com/simple/`

`pip install xxx -r req.tx -i https://pypi.doubanio.com/simple/`

中科大源    `http://pypi.mirrors.ustc.edu.cn/`   

清华源  `https://mirrors.tuna.tsinghua.edu.cn/`

阿里源  `http://mirrors.aliyun.com/`

`pip install pymysql -i http://pypi.zenlogic.net/simple/`

### pip的一些命令

`pip list --outdated`  #列出所有过期包

`pip install --upgrade [package]`  #升级指定包

`pip install pip-review`

`pip-review --local --interactive` #升级所有过期包

```
# 批量更新过期包
import pip
from subprocess import call

for dist in pip.get_installed_distributions():
    call('pip install --upgrade ' + dist.project_name, shell=True)
```

### 发布自己的python包流程
```
# 目录结构
my_module/
    LICENSE.rst
    README.rst
    setup.py
    setup.cfg
    my_module/
        __init__.py

# setup.py
from setuptools import setup

setup(
  name = 'my module',
  version = '0.1.0',
  packages = ['my_module',],
  author = 'tony',
  author_email = '',
  license = 'MIT',
  long_description = open('README.rst').read(),
  extras_require = {

  }
)

# 压缩打包
python setup.py sdist

# 发布模块
python setup.py register
# 上传到pypi
python setup.py sdist upload
```


## 4. gem源

`gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/`

`gem sources -l`

`gem install rails`

<br/><br/><br/>


# Mac环境配置

## 安装brew

`curl -LsSf http://github.com/mxcl/homebrew/tarball/master | sudo tar xvz -C /usr/local --strip 1`

或者

`ruby -e "$(curl --insecure -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

若需要卸载

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"`

`sudo brew doctor` #检查是否正常

`sudo brew install wget`

`sudo brew uninstall wget`

`sudo brew search /python*/` #搜索, 支持正则

`sudo brew services start|stop|restart [service]`

## 替换homebrew源

**替换brew.git**

`cd "$(brew --repo)"`

`git remote set-url origin https://mirrors.ustc.edu.cn/brew.git`

`git remote set-url origin https://github.com/Homebrew/brew.git` #切回官方源

**替换homebrew-core.git**

`cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"`

`git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git`

`git remote set-url origin https://github.com/Homebrew/homebrew-core.git` #切回官方源

<br/>

## 环境变量相关

**安装服务**

`brew install mongodb/redis/nginx/mysql/php/python/python3/go/rabbitmq`

[mac下nmp环境搭建](https://blog.frd.mn/install-nginx-php-fpm-mysql-and-phpmyadmin-on-os-x-mavericks-using-homebrew/)

`ln -sfv /usr/local/Cellar/[service_name]/[version_name]/homebrew.mxcl.[service_name].plist ~/Library/LaunchAgents` #建立软链

rabbitmq服务管理

`sudo rabbitmq-server [-detached]` #启动服务

`sudo rabbitmqctl stop` #停止服务

`sudo scutil --set HostName work` #设置hostname

pytesseract系统依赖

`brew install leptonica tesseract`

`brew install php56-mongo [--build-from-source]` # 安装php-mongo扩展

<br/>

**mac取消sudo密码**

`sudo visudo`

`%admin ALL = (ALL) NOPASSWD: ALL` # 找到并修改成这样

`~/.bash_aliases`  #别名管理

```
alias nginx.start='sudo launchctl load ~/Library/LaunchDaemons/homebrew.mxcl.nginx.plist'
alias nginx.stop='sudo launchctl unload ~/Library/LaunchDaemons/homebrew.mxcl.nginx.plist'
alias nginx.restart='nginx.stop && nginx.start'

alias php-fpm.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.php56.plist"
alias php-fpm.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.php56.plist"
alias php-fpm.restart='php-fpm.stop && php-fpm.start'

alias mongod.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist"
alias mongod.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist"
alias mongod.restart='mongod.stop && mongod.start'

alias redis.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist"
alias redis.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist"
alias redis.restart='redis.stop && redis.start'

alias mysql.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist"
alias mysql.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist"
alias mysql.restart='mysql.stop && mysql.start'

alias nginx.logs.error='tail -250f /usr/local/etc/nginx/logs/error.log'
alias nginx.logs.access='tail -250f /usr/local/etc/nginx/logs/access.log'
alias nginx.logs.default.access='tail -250f /usr/local/etc/nginx/logs/default.access.log'
alias nginx.logs.default-ssl.access='tail -250f /usr/local/etc/nginx/logs/default-ssl.access.log'

alias sub1="open -a Sublime\ Text"
alias ll='ls -al'
```
<br/>

`~/.bash_profile`

```
export PATH="/usr/local/sbin:$PATH"
PHP_AUTOCONF="/usr/local/bin/autoconf"
export LS_OPTIONS='--color=auto'
export CLICOLOR='Yes'
export LSCOLORS='Exfxcxdxbxegedabagacad'
source ~/.bash_aliases
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
```
<br/>

`~/.zshrc`

```
export PATH="/usr/local/sbin:$PATH"
PHP_AUTOCONF="/usr/local/bin/autoconf"
source ~/.bash_aliases
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
```
<br/>


## golang 环境变量设置

`go env`

`vim ~/.bash_profile`

```
export GOPATH=/usr/local/Cellar/go/1.8.3
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN
```

解决从`golang.org`下载依赖失败的问题

`mkdir -p $GOPATH/src/golang.org/x`

`cd $GOPATH/src/golang.org/x && git clone https://github.com/golang/net.git`


## 编译安装pkgconfig

`curl http://pkgconfig.freedesktop.org/releases/pkg-config-0.29.1.tar.gz -o pkgconfig.tg.gz`

`tar -zxf pkgconfig.tg.gz && cd pkg-config-0.29.1`

`./configure && make install`

`shift + command + G` # 进入系统隐藏目录

`shift + command + 4` # 截图

`sudo scutil --set HostName work` # 设置hostname
