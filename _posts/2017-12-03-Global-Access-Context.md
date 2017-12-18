
---

layout: post  

title: Global Access Context

date: 2017-12-03 03:00:00 +0800 

categories: AS  

---

## 1、新建MyApplication类，继承Application

```java
public class MyApplication extends Application {
    private static Context context;

    @Override
    public void onCreate() {
        context=getApplicationContext();
    }
    public static Context getContext(){
        return context;
    }
}
```



## 2、AndroidMainfest.xml内<application>标签内添加一行

```xml
<application    
android:name=".MyApplication"
```

## 3、项目任何地方要使用context，则

```java
Toast.makeText(MyApplication.getContext(),"network is unavailable",Toast.LENGTH_SHORT).show();
```

## 4、针对litepal配置已经添加了android:name="org.litepal.LitePalApplication"

## 则需要在自己的Application中调用litepal的初始化方法

```java
public class MyApplication extends Application{
     private static Context context;
     public void onCreate(){
      context=getApplicationContext();
      LitePalApplication.initialize(context);  //
      }
      public static Context getContext(){
      return context;
      }
}
```

Markdown**
*Awesome*