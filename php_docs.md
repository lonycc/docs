### php的一些安全配置

#### 0x01 php.ini配置项

`expose_php = Off` #大概375行, 禁止显示x-powerd-by信息

`disable_functions = eval,phpinfo,passthru,exec,system,chroot,scandir,chgrp,chown,shell_exec,proc_open,proc_get_status,error_log,ini_alter,ini_set,ini_restore,dl,pfsockopen,popen,symlink,readlink,putenv,stream_socket_server,file_get_contents,file_put_contents,syslog` #大概第303行，禁用的内置方法

`error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT` #大概第449行，禁止所有级别错误信息报告

`display_errors = Off` #大概第466行，禁止显示错误

`display_startup_errors = Off`  #大概第473行

`track_errors = Off` #大概第517行，禁止错误信息追踪

`register_argc_argv = Off` #大概第633行

`allow_url_fopen = Off`  #大概809行，禁止远程打开

`allow_url_include = Off` #大概813行，禁止远程包含

`zend.assertions = -1`  #大概第1528行，php7有，php55没有


#### 0x02 上传附件（图片为例）

```
<?php
/**
  +------------------------------------------------------------------------------
* Upload 文件上传类
  +------------------------------------------------------------------------------
* @package   Upload
  +------------------------------------------------------------------------------
*/
class Upload {
      private static $image = null;
      private static $status = 0;
      private static $suffix = null;
      private static $imageType = array('.jpg', '.bmp','.gif','.png');　　//允许的图片类型
      private static $message = array(　　//文件上传错误信息
            '0' => '没有错误发生，文件上传成功。',
            '1' => '上传的文件超过了 php.ini 中 upload_max_filesize 选项限制的值。',
            '2' => '上传文件的大小超过了 HTML 表单中 MAX_FILE_SIZE 选项指定的值。',
            '3' => '文件只有部分被上传。',
            '4' => '没有文件上传。',
            '5' => '未能通过安全检查的文件。',
            '6' => '找不到临时文件夹。',
            '7' => '文件写入失败。',
            '8' => '文件类型不支持',
            '9' => '上传的临时文件丢失。',
      );
      //@ 开始执行文件上传
      public static function start($feild = 'file') {
            if (!empty($_FILES)) {
                self::$status = $_FILES[$feild]['error'];
               if (self::$status > 0)
                      return array('status' => self::$status, 'msg' => self::$message[self::$status]);
                  self::$image = $_FILES[$feild]['tmp_name'];
                  self::$suffix = strtolower(strrchr($_FILES[$feild]['name'], '.'));
                  return array('status' => self::_upload(), 'path' => self::$image, 'msg' => self::$message[self::$status]);
           } else {
                  return array('status' => self::$status, 'msg' => self::$message[self::$status]);
           }
      }
    //@ 私有 上传开始
    private static function _upload($path = './upload/') {
        date_default_timezone_set('PRC');
        $newFile = $path . date('Y/m/d/His') . rand(100, 999) . self::$suffix;　　//定义上传子目录
        self::umkdir(dirname($newFile));
        if (is_uploaded_file(self::$image) && move_uploaded_file(self::$image, $newFile)) {
            self::$image = $newFile;　　　　　　　　　　　　　　　　// 生成的新文件名
            if (in_array(self::$suffix, self::$imageType))　　　// 判断上传类型是否符合规定
                return self::checkHex();　　　　　　　　　　　　　// 返回木马脚本检测的返回值
            else
                return self::$status = 0;
        } else {
            return self::$status = 9;
        }
    }
    //@ 私有 16进制检测
    private static function checkHex() {
        if (file_exists(self::$image)) {
            $resource = fopen(self::$image, 'rb');
            $fileSize = filesize(self::$image);
            fseek($resource, 0);　　　//把文件指针移到文件的开头
            if ($fileSize > 512) { // 若文件大于521B文件取头和尾
                $hexCode = bin2hex(fread($resource, 512));
                fseek($resource, $fileSize - 512);　　//把文件指针移到文件尾部
                $hexCode .= bin2hex(fread($resource, 512));
            } else { // 取全部
                $hexCode = bin2hex(fread($resource, $fileSize));
            }
            fclose($resource);
            /* 匹配16进制中的 <% ( ) %> */
            /* 匹配16进制中的 <? ( ) ?> */
            /* 匹配16进制中的 <script | /script> 大小写亦可*/

　　　　　　/* 核心  整个类检测木马脚本的核心在这里  通过匹配十六进制代码检测是否存在木马脚本*/

            if (preg_match("/(3c25.*?28.*?29.*?253e)|(3c3f.*?28.*?29.*?3f3e)|(3C534352495054)|(2F5343524950543E)|(3C736372697074)|(2F7363726970743E)/is", $hexCode))
                self::$status = 5;
            else
                self::$status = 0;
            return self::$status;
        } else {
            return self::$status = 9;
        }
    }
    //@ 私有 创建目录
    private static function umkdir($dir) {
        if (!file_exists($dir) && !is_dir($dir)) {
            self::umkdir(dirname($dir));
            @mkdir($dir);
        }
    }
}
```
> 对于上传的图片，永远不要把源文件提供给用户访问，可以用GD库处理生成缩略图（压缩），这样图片木马就无所遁形了。对于非图片附件，检测MIME-Type之余，再对文件做16进制检测。

```
# nginx 配置限制上传目录执行脚本
location ~* ^/(upload|data|doc|static)/ 
{
	location ~* .*\.(php|php5)$
	{
		deny all;
	}
}
```

# apache和nginx设置环境变量

## apache设置方式
```
# SetEnv用于设置环境变量
<VirtualHost *:80>
    ServerAdmin admin@admin.com
    DocumentRoot "/var/www/"
    ServerName localhost
    SetEnv RUNTIME_ENVIROMENT DEV
    SetEnv MYSQL_USERNAME root
    SetEnv MYSQL_PASSWORD root
    ErrorLog "logs/error.log"
    CustomLog "logs/access.log" common
</VirtualHost>
```

## nginx设置方式
```
# 在fastcgi_params文件中配置
fastcgi_param RUNTIME_ENVIROMENT 'DEV';
fastcgi_param MYSQL_USERNAME 'root';
fastcgi_param MYSQL_PASSWORD 'root';

# 然后在location下引入fastcgi_params
location ~ \.php$ {
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_index index.php;
    fastcgi_param  SCRIPT_FILENAME  /path/to/web$fastcgi_script_name;
    include fastcgi_params;
}
```

## php获取环境变量
```
$env = getenv('RUNTIME_ENVIROMENT');
或者
$env = $_SERVER['RUNTIME_ENVIROMENT'];
```


## php安装Oracle扩展

先从这里[oracle官网](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html)下载相应的客户端安装包(basic+devel).

`rpm -ivh oracle-instantclient-basic-version.arch.rpm`

`rpm -ivh oracle-instantclient-devel-version.arch.rpm`

`cd /usr/local/src/php-5.5.14/ext/oci8`

`/usr/local/php/bin/phpize`

`./configure --with-php-config=/usr/local/php/bin/php-config --with-oci8=/usr/lib/oracle/12.2/client64`

`make`

`make install`

> 如果`make`报错, 显示找不到库文件, 则手动修改`Makefile`, 找到`INCLUDES`, 在该行尾部加上`-I/usr/include/oracle/12.2/client64 -I/usr/lib/oracle/12.2/client64`, 再执行`make`.

> 修改`/usr/local/php/etc/php.ini`, 加上`extension=oci8.so`. 确保`/usr/local/php/lib/php/extensions/no-debug-non-zts-xxx`路径下有`oci8.so`.

> `pdo_oci`扩展的安装方式同上,  其所在路径为`/usr/local/src/php-5.5.14/pdo_oci`.

如果`pdo_oci`遇到字符集问题, 一个解决方案是先创建`/etc/profile.d/oracle.sh`

```
#!/bin/bash
ORACLE_HOME=/usr/lib/oracle/12.2/client64
LD_LIBRARY_PATH=$ORACLE_HOME/lib
NLS_LANG=american_america.utf8
export ORACLE_HOME LD_LIBRARY_PATH NLS_LANG
```

然后编辑`/etc/init.d/php-fpm`, 添加一行

`. /etc/profile.d/oracle.sh`

重启`php-fpm`即可.

<br><br/>

## php5.2 + fpm

`tar zxvf php-5.2.11.tar.gz`  # 先解压php源码包

`gzip -cd php-5.2.11-fpm-0.5.13.diff.gz | patch -d php-5.2.11 -p1`  # 将php-fpm以补丁方式注入php源码包

`cd php-5.2.11`

`./configure --with-pdo-mysql=/usr/local/mysql --prefix=/usr/local/php52  --with-gd=/usr/local/gd2  --with-jpeg-dir=/usr/local/jpeg6 --with-zlib --with-png-dir=/ --with-freetype-dir=/usr/local/freetype2 -enable-trace-vars --with-mysql=/usr/local/mysql --enable-mbstring=all --with-curl --enable-mbregex --with-config-file-path=/etc --enable-ftp --enable-soap --with-xsl --with-mcrypt --enable-zip --enable-fastcgi --enable-fpm --enable-force-CGI-redirect --with-gettext --enable-pcntl --enable-sockets --enable-bcmath`

`make && make install`

`cp php.ini-dist /usr/local/php52/lib/php.ini`

`/usr/local/php52/sbin/php-fpm start|stop|quit|restart|reload|logrotate`

> 注: 如果提示要重装`libcurl`, 只需`yum install -y curl-devel`. 提示找不到`gettext`库, 则安装`yum install -y gettext gettext-devel`.

<br/>

## 编译安装php的过程

依次编译安装`yasm`, `libmcrypt`, `libvpx`, `tiff`, `libpng`, `freetype`, `jpeg`, `libgd`, `tllib`, 其中`yasm`和`libmcrypt`模块直接`./configure`然后编译安装即可;其他模块配置参数如下:

`./configure --prefix=/usr/local/libvpx --enable-shared --enable-vp9`

`./configure --prefix=/usr/local/tiff --enable-shared`

`./configure --prefix=/usr/local/libpng --enable-shared`

`./configure --prefix=/usr/local/freetype --enable-shared`

`./configure --prefix=/usr/local/jpeg --enable-shared`

`./configure --prefix=/usr/local/libgd --enable-shared --with-jpeg=/usr/local/jpeg --with-png=/usr/local/libpng --with-freetype=/usr/local/freetype --with-fontconfig=/usr/local/fontconfig --with-xpm=/usr/lib64 --with-tiff=/usr/local/tiff --with-vpx=/usr/local/libvpx`

`./configure --prefix=/usr/local/t1lib --enable-shared`

> 注: 如果编译`jpeg`时提示找不到`./libtool`, 则先`yum -y install libtool libtool-ltdl-devel`, 然后`cp /usr/share/libtool/config/config.sub .`以及`cp /usr/share/libtool/config/config.guess .`完成替换操作, 然后重新编译.

> 如果编译`jpeg`时提示`cannot create regular file '/usr/local/jpeg/include/jconfig.h'`, 那就手工建目录`mkdir -p /usr/local/jpeg/man/man1 /usr/local/jpeg/include /usr/local/jpeg/bin /usr/local/jpeg/lib`.

> 注: 编译`t1lib`时略有些不同, 需要`make without_doc && make install`

> 注: 编译`libgd`时提示`fontconfig support requested, but not found`, 则需要先编译安装`fontconfig`, 编译参数为`./configure --prefix=/usr/local/fontconfig --enable-libxml2`. 在此之前要先`yum -y install freetype-devel libxml2 libxml2-devel libXpm libXpm-devel`.

> 注: 如果`libgd`实在编译不过, 那就跳过这一步, 使用系统默认的. 只需要在编译php的时候将`--with-gd=/usr/local/libgd`改为`--with-gd`即可.

对于64位系统, 需要执行两条命令如下:

`\cp -frp /usr/lib64/libltdl.so* /usr/lib/`

`\cp -frp /usr/lib64/libXpm.so* /usr/lib/`


## [php-fpm配置与优化](https://www.zybuluo.com/phper/note/89081)

**安装ldap模块**
```
yum install openldap
yum install openldap-devel

# 进入php源码目录
cd php-7.1.1/ext/ldap

# 运行phpize
/usr/local/php/bin/phpize

# 编译安装
./configure --with-ldap --with-php-config=/usr/local/php/bin/php-config
make
make install
ls -l /usr/local/php/lib/php/extensions/no-debug-non-zts-xxx

# php.ini配置文件增加ldap模块
vi /usr/local/php/etc/php.ini
extension="ldap.so"

# 重启php-fpm
service php-fpm restart
```

**php-fpm启动/重启/停止**

```
INT, TERM 立刻终止
QUIT 平滑终止
USR1 重新打开日志文件
USR2 平滑重载所有worker进程并重新载入配置和二进制模块

kill -INT `cat /var/run/php-fpm.pid`  #停止
kill -USR2 `cat /var/run/php-fpm.pid` #重启
```
