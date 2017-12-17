---

layout: post  

title: Hierarchical Navigation

date: 2017-12-03 06:00:00 +0800 

categories: AS  

---

## 1、AndroidManifest.xml添加属性，开启层级式导航

```XML
<activity android:name=".CrimePagerActivity"
    android:parentActivityName=".CrimeListActivity">
</activity>
```

## 2、用户在CrimePagerActivity界面向上导航时，如下intent被创建

Intent intent=new Intent(this,CrimeListActivity.class);

intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);   

//FLAG_ACTIVITY_CLEAR_TOP指示Android在回退栈中寻找特定的activity实例，如果实例存在，则弹出栈内所有其他activity，让启动的目标activity出现在栈顶。

startActivity(intent);

finish();

## 3、缺陷

导航回退到的目标activity会被完全重建，实例变量值和保存的实例状态全部丢失

解决方法

1、finish()回退，但只能回退前一个

2、将需要的信息作为extra信息传递到新Activity中


## 



**Markdown**
*Awesome*