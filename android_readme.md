## Activity: 活动

> 最基本的Android应用程序组件。应用程序中，一个活动通常就是一个单独的屏幕。每一个活动都被实现为一个独立的类，并且从活动基类中继承而来，活动类将会显示由视图控件组成的用户接口，并对事件做出响应。基类是android.app.Activity，其他Activity继承该父类，通过Override父类的方法来实现各种功能。

## Intent：调用Android 专有类Intent进行架构屏幕之间的切换

> Intent 是描述应用想要做什么。Intent数据结构两个最重要的部分是动作和动作对应的数据。典型的动作类型有:MAIN（活动的门户）、VIEW、PICK、EDIT 等。而动作对应的数据则以URI的形式进行表示。

### IntentFilter：用于描述一个activity能够操作哪些intent。

> 一个 activity 如果要显示一个人的联系方式时，需 要声明一个 IntentFilter，这个 IntentFilter 要知道怎么去处理 VIEW 动作和表示一个人的 URI。IntentFilter 需 要在 AndroidManifest.xml 中定义。

### IntentReceiver:但你希望你的应用能够对一个外部的事件做出响应，你可以使用一个IntentReceiver。

## Service：一个Service是一段长生命周期的，没有GUI的程序。

> 例如媒体播放器这个activity会使用Context.startService()来启动一个service，从而可以在后台保持音乐的播放。

## ContentProvider:应用数据保存在文件，SQLite数据库等，用于数据共享，就需要内容提供器。

> Android中每一个程序运行在各自的进程中，当一个应用要访问其他应用的数据时，也就需要数据在不同的虚拟机之间传递。ContentProvider就是用来解决在不同的应用包之间共享数据的工具。
安卓本地ContentProvider包括：CallLog、Contact.People.Phones、Setting.System

## Android 虚拟机 Dalvik

> Dalvik基于寄存器，JVM基于栈。对于更大的程序，Dalvik编译时间更短。 Dalvik 经过优化，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个 Dalvik 应用作为一个独立的 Linux 进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。

Android提供了一些扩展的Java类库，若要引用基础类库，一般import javax.swing.*;

## 重要的包描述：

android.app: 提供高层的程序模型，提供基本的运行环境

android.content: 包含各种的对设备上的数据进行访问和发布的类

android.database: 通过内容提供者浏览和操作数据库

android.graphics: 底层的图形库，包含画布，颜色过滤，点，矩形，可绘制在屏幕上

android.location: 定位和相关服务的类

android.media：提供一些类管理多种音频、视频的媒体接口

android.net：提供帮助网络访问的类，超过通常的java.net.*接口

android.os：提供系统服务、消息传输、IPC机制

android.opengl: 提供OpenGL的工具

android.provider: 提供类访问android的内容提供者

android.telephony: 提供与拨打电话相关的API交互

android.view：提供基础的用户界面接口框架

android.util: 涉及工具性的方法，例如时间日期的操作

android.webkit: 默认浏览器操作接口

android.widget: 包含各种UI元素，在应用程序屏幕中使用

 
## Android的相关文件类型

### java文件--------------应用程序源文件

### class文件-------------Java编译后的目标文件
  安卓平台上的class文件不能直接运行，因为Google使用了自己的Dalvik来运行应用。class文件只是编译过程中的中间目标文件，需要链接成dex文件后才能在dalvik上运行。
### Dex文件---------------Android平台上的可执行文件
  安卓虚拟机Dalvik支持的字节码文件格式，并非Java字节码，而是dex格式的字节码。java字节码可通过工具转换成Dex字节码。

### Apk文件--------------Android上的安装文件
   将一个应用程序相关的所有文件，如AndroidManifest.xml 文件、应用程序代码(.dex 文件)、资源文件和其他文件打成一个压缩包。
 
## Android应用程序结构分析

Android.mk   整个工程的Makefile

AndroidManifest.xml  工程的描述文件，运行时有用

res:  放置资源文件的目录

编译的中间结果

classes.dex

classes.jar

目标apk文件
 
## Android ADB工具使用

通用调试工具，管理模拟器或设备。

`adb install app.apk`  #安装应用到模拟器

`adb shell`  #进入模拟器的Shell

`cd data/app`

`rm app.apk`  #卸载应用

`adb shell [command]`  #执行一条shell命令
 
### 发布端口，可以设置任意的端口号，作为主机向模拟器或设备的请求接口。
`adb forward tcp:5555 tcp:8000`

### 复制文件，可向一个设备或从一个设备中复制文件.
`adb push  test.txt /tmp/test.txt`  #复制文件到模拟器中

`adb pull   /android/lib/libwebcore.os`  #从模拟器中复制一个文件或目录

### 搜索/等待模拟器、设备实例
`adb  devices`

`adb  wait-for-device`

`adb  bugreport`  #查看bug报告
 
`adb  shell`

`sqlite3` #访问数据库SQLite3

`logcat  -b  radio`  #记录无线通讯日志

`adb  get-product` #获取设备ID

`adb  get-serialno`  #获取设备序列号
 
 
## Android模拟器
参数格式 `emulator  [option]  [-qemu args]`

### 进程：

> 在安卓中，进程完全是应用程序的实现细节，不是用户一般想象的那样。

### 线程：

> 安卓让一个应用程序在单独的线程中，指导它创建自己的线程

#### 进程优先级：

- 1、前台进程：包含一个前台Activity、一个正在运行的广播接收器、正在运行的服务

- 2、可视进程：包含一个可视的Activity

- 3、服务进程：包含一个被开启的服务

- 4、后台进程：包含一个不可视的Activity

- 5、空进程：没有持有任何应用程序组件

## R.layout:  指向用户项目res目录下的layout

## android.R.layout:  指向Android SDK自带的布局目录

**http://www.cnblogs.com/greatverve/archive/2011/12/28/android-dip-dp-sp-pt-px.html**

dip: device independent pixels 设备独立像素

dp: 同dip

px: pixels 像素

pt: point 1pt=1/72英寸

sp: scaled pixels 放大像素，主要用于字体显示

in: 英寸

mm: 毫米

## AndroidManifest.xml
```
<manifest>
xmlns:android="http://schemas.android.com/apk/res/android" #命名空间
package #当前应用的包名
android:versionCode #当前应用版本(整数)
android:versionName #显示给用户的版本号
android:installLocation #应用安装位置，可选preferExternal或auto

<uses-sdk>
android:minSdkVersion  #指定最低SDK版本，默认是1，19对应4.4
android:maxSdkVersion  #最顶最高SDK版本，一般不指定
android:targetSdkVersion  #目标SDK版本，最佳版本

<uses-permission>
android:name="android.permission.INTERNET"  #应用所需权限

<uses-feature>
android:name #硬件或软件资源名
android:required=["true" | "false"] #是否必须
android:glEsVersion="integer"

<instrumentation>
android:name="android.test.InstrumentationTestRunner"  #单元测试工具
android:targetPackage  #指定要测试的包名

<application>
	<activity>
		<intent-filter> #意图过滤器
			<action android:name="string" /> #动作有系统自带的，也可以自定义
			<category android:name="string" /> #类别
			<data android:scheme="" android:mimeType="string" /> #数据类型定义
```
