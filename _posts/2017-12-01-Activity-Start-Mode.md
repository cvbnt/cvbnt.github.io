
---  
layout: post  
title: Activity-Start-Mode
date: 
categories: AS  
---  

## 任务栈

 &nbsp;&nbsp;任务栈Task，是一种用来放置Activity实例的容器，他是以栈的形式进行盛放，也就是所谓的先进后出，主要有2个基本操作：压栈和出栈，其所存放的Activity是不支持重新排序的，只能根据压栈和出栈操作更改Activity的顺序。  

 &nbsp;&nbsp;启动一个Application的时候，系统会为它默认创建一个对应的Task，用来放置根Activity。默认启动Activity会放在同一个Task中，新启动的Activity会被压入启动它的那个Activity的栈中，并且显示它。当用户按下回退键时，这个Activity就会被弹出栈，按下Home键回到桌面，再启动另一个应用，这时候之前那个Task就被移到后台，成为后台任务栈，而刚启动的那个Task就被调到前台，成为前台任务栈，Android系统显示的就是前台任务栈中的Top实例Activity。  

## standard

默认模式，可以不用写配置。在这个模式下，都会默认创建一个新的实例。因此，在这种模式下，可以有多个相同的实例，也允许多个相同Activity叠加。应用场景：绝大多数Activity。
![standard](https://cvbnt.github.io/cvbnt.github.io/assets/images/standard.png)
如果以这种方式启动的Activity被跨进程调用，在5.0之前新启动的Activity实例会放入发送Intent的Task的栈的顶部，尽管它们属于不同的程序，这似乎有点费解看起来也不是那么合理，所以在5.0之后，上述情景会创建一个新的Task，新启动的Activity就会放入刚创建的Task中，这样就合理的多了。

## singleTop

栈顶复用模式，如果要开启的activity在任务栈的顶部已经存在，就不会创建新的实例，而是调用 onNewIntent() 方法。避免栈顶的activity被重复的创建。应用场景：在通知栏点击收到的通知，然后需要启动一个Activity，这个Activity就可以用singleTop，否则每次点击都会新建一个Activity。
manifest.xml添加
```XML
<activity android:name=".MainActivity"    
android:launchMode="singleTop">
```
当活动在栈顶时不会重新创建活动 
![standard](https://cvbnt.github.io/cvbnt.github.io/assets/images/singleTop.png)

## singleTask

栈内复用模式， activity只会在任务栈里面存在一个实例。如果要激活的activity，在任务栈里面已经存在，就不会创建新的activity，而是复用这个已经存在的activity，调用 onNewIntent() 方法，并且清空这个activity任务栈上面所有的activity。应用场景：大多数App的主页。对于大部分应用，当我们在主界面点击回退按钮的时候都是退出应用，那么当我们第一次进入主界面之后，主界面位于栈底，以后不管我们打开了多少个Activity，只要我们再次回到主界面，都应该使用将主界面Activity上所有的Activity移除的方式来让主界面Activity处于栈顶，而不是往栈顶新加一个主界面Activity的实例，通过这种方式能够保证退出应用时所有的Activity都能报销毁。
在跨应用Intent传递时，如果系统中不存在singleTask Activity的实例，那么将创建一个新的Task，然后创建SingleTask Activity的实例，将其放入新的Task中。

1：假如目前有个任务栈T1中的情况是ABC，这个时候ActivityD以singleTask模式请求启动，其所需要的任务栈正是T1，则系统会直接创建D的实例并将其入栈到T1中。
![singleTask2](https://cvbnt.github.io/cvbnt.github.io/assets/images/singleTask2.png)
2：假如DActivity启动所需要的任务栈为T2,由于T2和D的实例均不存在，那么系统会先创建任务栈T2，然后再创建D的实例并将其入栈到T2中。我们可以通过设置Activity的taskAffinity属性来模拟这一场景。
```XML
<activity 
android:name=".SingleTaskActivity" 
android:label="singleTask launchMode" 
android:launchMode="singleTask" 
android:taskAffinity="">
</activity>
```
![singleTask3](https://cvbnt.github.io/cvbnt.github.io/assets/images/singleTask3.png)
3：如果D所需的任务栈为T3，并且当前任务栈T3的情况为ADBC，根据栈内复用的原则，此时D不会重新创建，系统会把D切换到栈顶并调用其onNewIntent()方法，同时由于singleTask默认具有ClearTop的效果，会导致栈内所有在D上面的Activity全部出栈，于是最终T3的情况为AD。
![singleTask4](https://cvbnt.github.io/cvbnt.github.io/assets/images/singleTask4.png)
4：假如目前有两个任务栈，前台任务栈T4的情况为AB,后台任务栈t4里存有CD,假设CD的启动模式均为singleTask，现在由B去启动D,那么整个后台任务都会被切换到前台，这个时候整个栈就变成了ABCD。
![singleTask5](https://cvbnt.github.io/cvbnt.github.io/assets/images/singleTask5.png)
5:假如上面的其他条件不变，B启动的是C而不是D,那么整个栈的情况就变成了ABC,因为D在C上面，会被清理出栈。
![singleTask5](https://cvbnt.github.io/cvbnt.github.io/assets/images/singleTask6.png)

## singleInstance
单一实例模式，整个手机操作系统里面只有一个实例存在。不同的应用去打开这个activity 共享公用的同一个activity。他会运行在自己单独，独立的任务栈里面，并且任务栈里面只有他一个实例存在。应用场景：呼叫来电界面。这种模式的使用情况比较罕见，在Launcher中可能使用。或者你确定你需要使Activity只有一个实例。建议谨慎使用.

## 设置Intent的Flag

系统提供了两种方式来设置一个Activity的启动模式，除了在AndroidManifest文件中设置以外，还可以通过Intent的Flag来设置一个Activity的启动模式，下面我们在简单介绍下一些Flag。  

FLAG_ACTIVITY_NEW_TASK  

使用一个新的Task来启动一个Activity，但启动的每个Activity都讲在一个新的Task中。该Flag通常使用在从Service中启动Activity的场景，由于Service中并不存在Activity栈，所以使用该Flag来创建一个新的Activity栈，并创建新的Activity实例。  

FLAG_ACTIVITY_SINGLE_TOP  

使用singletop模式启动一个Activity，与指定android：launchMode=“singleTop”效果相同。  

FLAG_ACTIVITY_CLEAR_TOP  

使用SingleTask模式来启动一个Activity，与指定android：launchMode=“singleTask”效果相同。  

FLAG_ACTIVITY_NO_HISTORY  

Activity使用这种模式启动Activity，当该Activity启动其他Activity后，该Activity就消失了，不会保留在Activity栈中。  

LaunchMode与StartActivityForResult  

我们在开发过程中经常会用到StartActivityForResult方法启动一个Activity，然后在onActivityResult()方法中可以接收到上个页面的回传值，但你有可能遇到过拿不到返回值的情况，那有可能是因为Activity的LaunchMode设置为了singleTask。5.0之后，android的LaunchMode与StartActivityForResult的关系发生了一些改变。两个Activity，A和B，现在由A页面跳转到B页面，看一下LaunchMode与StartActivityForResult之间的关系：  
![beforce5.0](https://cvbnt.github.io/cvbnt.github.io/assets/images/beforce5.0.png)
![after5.0](https://cvbnt.github.io/cvbnt.github.io/assets/images/after5.0.png)
这是因为ActivityStackSupervisor类中的startActivityUncheckedLocked方法在5.0中进行了修改。在5.0之前，当启动一个Activity时，系统将首先检查Activity的launchMode，如果为A页面设置为SingleInstance或者B页面设置为singleTask或者singleInstance,则会在LaunchFlags中加入FLAG_ACTIVITY_NEW_TASK标志，而如果含有FLAG_ACTIVITY_NEW_TASK标志的话，onActivityResult将会立即接收到一个cancle的信息，而5.0之后这个方法做了修改，修改之后即便启动的页面设置launchMode为singleTask或singleInstance，onActivityResult依旧可以正常工作，也就是说无论设置哪种启动方式，StartActivityForResult和onActivityResult()这一组合都是有效的。所以如果你目前正好基于5.0做相关开发，不要忘了向下兼容，这里有个坑请注意避让。


**Markdown**
*Awesome*