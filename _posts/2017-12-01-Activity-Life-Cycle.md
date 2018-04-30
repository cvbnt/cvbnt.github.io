---
layout: post  
title: Activity Life Cycle
date: 2017-12-01 19:00:00 +0800 
categories: AS  
---  

## 1、Activity生命周期
![image0](https://cvbnt.github.io/cvbnt.github.io/assets/images/Activity-Life-Cycle.jpg)

打开应用程序：

onCreate() --> onStart() -->  onResume()

当后退按钮按下并退出应用程序：

onPaused() -- > onStop() --> onDestory()

当主页按钮被按下时：

onPaused() --> onStop()

当再次从最近的任务列表打开应用程序或点击图标时按下按下主页按钮：

onRestart() --> onStart() --> onResume()

当通过通知栏打开应用程序或打开设置：

onPaused() --> onStop()

从其他应用程序或设置中按下后退按钮看到我们的应用程序：

onRestart() --> onStart() --> onResume()

当任何对话框再屏幕上打开

onPause()

关闭对话框中的对话框或返回按钮后：

onResume()

手机响铃，用户在应用程序中：

onPause() --> onResume() 

当用户按下电话应答按钮：

onPause()

通话结束：

onResume()

当手机屏幕关闭

onPaused() --> onStop()

当屏幕重新打开

onRestart() --> onStart() --> onResume()

## 2、分屏情况下的生命周期

当前显示自己的应用页面，长按多任务键时出现分屏：

**onMultiWindowModeChanged（true）** - > onPause-onStop-> onDestroy-> onCreate-> onStart-> onResume-> onPause

分屏时长按多任务键，全屏显示自己的应用：

onStop-> onDestroy-> onCreate-> onStart-> onResume> onPause> **onMultiWindowModeChanged（false）** - > onResume

当前显示其他应用，按多任务键出现自己的应用时：

**onMultiWindowModeChanged（true）** - > nDestroy-> onCreate-> onStart-> onResume

## 3、其他生命周期

![image1](https://cvbnt.github.io/cvbnt.github.io/assets/images/Life-Cycle-Full.png)




**Markdown**
*Awesome*