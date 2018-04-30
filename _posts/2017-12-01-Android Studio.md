---
header:  
  teaser: /assets/images/Android Studio.png  
layout: post  
title: Application Debug  
date: 2017-12-01 09:00:00 +0800  
categories: AS  
search：true
---  

## Android Studio配置

[下载地址](https://developer.android.com/studio/index.html)  
[预览版](https://developer.android.com/studio/preview/index.html)

## ADB的环境变量配置

1、计算机(Win7)\这台电脑(Win8)\此电脑(Win10) -> 右键 -> 属性 -> 高级系统设置 -> 高级 -> 环境变量 ->系统变量  
2、变量名:Android,变量值:D:\android-sdk\platform-tools  
3、path添加:D:\android-sdk\platform-tools  
4、cmd内运行adb  
[成功](http://images2015.cnblogs.com/blog/805379/201703/805379-20170316002855745-310289725.png)  

## Android Studio快捷键

1、ctrl+alt +s&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;打开设置  
2、ctrl+alt+shift+s&ensp;&ensp;&ensp;&ensp;&ensp;项目结构  
3、ctrl+[或]&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;跳转到最近的括号   
4、ctrl+F12&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;显示文件结构  
5、选中某个元素，ctrl+F7，查看引用，F3跳转下一个   
6、ctrl+N&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;快速打开类   
7、ctrl+shift+N&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;快速打开文件   
8、ctrl+shift+insert&ensp;&ensp;&ensp;&ensp;查看剪切板并插入   
9、alt+insert&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;生成构造器getter/setter   
10、选中内容，ctrl+alt+T，选择内容被哪个语句包住   
11、方法间快速移动，alt+?和alt+?   
12、注释ctrl+/ 选中内容注释ctrl+shift+/   
13、ctrl+E&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;显示最近打开文件   
14、设置里的Live Templates,自定义快捷键，例如：sop+Tap   
15、shift+F6&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;重命名  
16、ctrl+o&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;快速查找调用方法 

# 配置AS识别成员变量m前缀生成get和set方法

1、Settings->Edtior->Code Style->Java->Code Generation   
2、Naming，Field:name prefix:m，static Field:name prefix:s  

## 重启虚拟机

控制台adb reboot

## 加大Android Studio利用运存

1、进入Android Studio所在文件夹，/bin

找到studio64.vmoptions文件，编辑成

```
-Xms2048m
-Xmx2048m
-XX:ReservedCodeCacheSize=1024m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djna.nosys=true
-Djna.boot.library.path=

-da
```

2、Android Studio显示Heap占用

Settings->Appearance，打开 Show memory indicator 选项

3、生效

File--Invalidate caches/restart

Markdown**
*Awesome*