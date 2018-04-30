---

layout: post  

title: Notification

date: 2017-12-02 10:00:00 +0800 

categories: AS  Notification

---

## 基本

1、NotificationManager用来管理通知,调用Context的getSystemService()方法获取，getSystemService()方法接收一个字符串参数用于确定获取系统的哪个服务

```Java
NotificationManager manager=(NotificationManager)getSystemService(NOTIFICATION_SERVICE);
```

​                                                                          ⬆默认是这个

  2、使用一个builder构造器来构建Notification对象

```java
Notification notification=new NotificationCompat.Builder(this)          
  .setContentTitle("This is content title")              //指定通知的标题内容        
  .setContentText("This is content text")                //指定通知正文内容        
  .setWhen(System.currentTimeMillis())                   //指定通知创建时间，毫秒单位        
  .setSmallIcon(R.mipmap.ic_launcher)                    //指定通知小图标（状态栏）        
  .setLargeIcon(BitmapFactory.decodeResource(getResources(),R.mipmap.ic_launcher)) //大图标        
  .build();                                              //创建
```

3、显示通知

```java
manager.notify(1,notification);
```

第一个参数1为通知id,第二个参数为通知对象 

# PendingIntent

相当于于延迟执行的Intent

效果，点击通知跳转activity

```java
public void onClick(View v){    
  switch (v.getId()){        
    case R.id.send_notice:            
      Intent intent=new Intent(this,NotificationActivity.class);    //创建intent      
      PendingIntent pi=PendingIntent.getActivity(this,0,intent,0);    //将intent传入PendingIntent的getActivity()方法里，得到PentingIntent的实例
            //参数1:context 参数2:一般为0 参数3:intent对象 参数4:用于确定PendingIntent的行为，通常为0      
      NotificationManager manager=(NotificationManager)getSystemService(NOTIFICATION_SERVICE);           Notification notification=new NotificationCompat.Builder(this)          
        .setContentTitle("This is content title")             
        .setContentText("This is content text")       
        .setWhen(System.currentTimeMillis())        
        .setSmallIcon(R.mipmap.ic_launcher)         
        .setLargeIcon(BitmapFactory.decodeResource(getResources(),R.mipmap.ic_launcher))      
        .setContentIntent(pi)                  //方法接收PendingIntent对象，构建出意图       
        .build();     
      manager.notify(1,notification);         
      break;      
    default:       
      break;   
  }
}
```

 

# 点击通知后让通知消失

1、

```java
Notification notification=new NotificatiobCompat.Builder(this)
  ...
  .setAutoCancel(true)
  .build();
```

2、

```java
在要跳转的activity内写
NotificationManager manager=(NotificationManager)getSystem.Servicemanager.cancel(1);
```

原先通知id为1

# 发出通知时播放音频

```java
Notification notification=new NotificatiobCompat.Builder(this) 
  ...
  .setSound(Uri.fromFile(new File("/system/media/audio/ringtones/Luna.ogg")))   
   ⬆音频路径
  .build();
```

# 发出通知时振动

```java
Notification notification=new NotificatiobCompat.Builder(this) 
  ...
  .setVibrate(new long[]{0,1000,1000,1000})
   ⬆以毫秒为单位，数组变量0表示手机静止时长，1表示手机振动时长，2表示手机静止时长，以上表示通知来时手机立刻振动1秒，静止1秒，再振动1秒
  .build();
```

并声明权限

```xml
 <user-permission android:name="android.permission.VIBRATE"/>
```

# 发出通知时控制LED灯闪烁

```java
Notification notification=new NotificatiobCompat.Builder(this) 
  ...
  .setLights(Clolr.GREEN,1000,1000)
   ⬆参数1颜色，参数2指定亮起的时长，参数3俺去的时长
总的效果是绿光反复闪烁
  .build();

默认效果
.setDefaults(NotificationCompat.DEFAULT_ALL)
```

# 通知显示长文字

```java
显示文字
Notification notification=new NotificatiobCompat.Builder(this) 
  ...
  .setContentText("Learn how to build notifications,send and sync data,and use voice actions")   
   ⬆文字过长无法完全显示
  .build();
  
显示长文字
Notification notification=new NotificatiobCompat.Builder(this) 
  ...
  .setStyle(new NotificationCompat.BigTextStyle().bigText("("Learn how to build notifications,send and sync data,and use voice actions")   
   ⬆setStyle()方法创建NotificationCompat.BigTextStyle()对象，该对象封装长文字，调用它的.bigText()方法将文字传入
  .build();
```

# 通知显示图片

```java
Notification notification=new NotificatiobCompat.Builder(this) 
  ...
  .setStyle(new NotificationCompat.BigTextStyle().bigPicture("BitmapFactory.decodeResouece(getResources(),R.drawable.big_image)))   
.build;
```

# 横幅通知（重要通知）

```java
Notification notification=new NotificatiobCompat.Builder(this) 
  ...
  .setPriority(NotificationCompat.PRIORITY_MAX) 
   ⬆5个等级，PRIORITY_DEFAULT(默认),PRIORITY_MIN(最低)，PRIORITY_LOW(较低),PRIORITY_HIGH(较高,系统会放大通知),PRIORITY_MAX(最高，会让用户立刻看到)
  .build();
```

## 自定义通知布局

自定义一个通知布局，通知布局仅接受以下组件(由RemoteViews决定)

`RemoteViews` 仅限于支持以下布局：

- `AdapterViewFlipper`
- `FrameLayout`
- `GridLayout`
- `GridView`
- `LinearLayout`
- `ListView`
- `RelativeLayout`
- `StackView`
- `ViewFlipper`

以下小部件：

- `AnalogClock`
- `Button`
- `Chronometer`
- `ImageButton`
- `ImageView`
- `ProgressBar`
- `TextClock`
- `TextView`


不支持这些类的子类

navigation_play.xml

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/layout"
    android:layout_width="match_parent"
    android:orientation="vertical"
    android:layout_height="100dp"
    android:padding="10dp" >

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"

            android:orientation="horizontal">
            <TextView
                android:id="@+id/navigation_song"
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="3"
                android:text="@string/not_play"
                android:gravity="center"
                android:textSize="20sp"
                />
            <TextView
                android:id="@+id/navigation_singer"
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                />
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:orientation="horizontal">

            <ImageButton
                android:id="@+id/navigation_previous"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:background="@android:color/transparent"
                app:srcCompat="@drawable/ic_skip_previous_black" />

            <ImageButton
                android:id="@+id/navigation_play"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:background="@android:color/transparent"
                app:srcCompat="@drawable/ic_play_arrow_black" />

            <ImageButton
                android:id="@+id/navigation_next"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:background="@android:color/transparent"
                app:srcCompat="@drawable/ic_skip_next_black" />
        </LinearLayout>
    </LinearLayout>
</RelativeLayout>
```

![navigaiton_play](https://cvbnt.github.io/cvbnt.github.io/assets/images/Navigation/navigation_play.PNG)

逻辑

```java
Notification notification;
    Notification.Builder builder;
    NotificationManager manager;
    RemoteViews mRemoteViews;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        context=this;
        mRemoteViews=new RemoteViews(getPackageName(),R.layout.navigation_play)；                     mRemoteViews.setImageViewResource(R.id.navigation_previous,R.drawable.ic_skip_previous_black);
        mRemoteViews.setImageViewResource(R.id.navigation_play,R.drawable.ic_play_arrow_black);
        mRemoteViews.setImageViewResource(R.id.navigation_next,R.drawable.ic_skip_next_black);
        mRemoteViews.setTextViewText(R.id.navigation_song,"未播放");
        builder=new Notification.Builder(this)
                .setContent(mRemoteViews)
                .setSmallIcon(R.mipmap.ic_launcher);
        notification=builder.build();
                notification.flags|=Notification.FLAG_AUTO_CANCEL;
        notification.defaults|=Notification.DEFAULT_SOUND;
        notification.defaults|=Notification.DEFAULT_VIBRATE;
        manager=(NotificationManager)getSystemService(NOTIFICATION_SERVICE);
        manager.notify(1,notification);
```

Markdown*
*Awesome*