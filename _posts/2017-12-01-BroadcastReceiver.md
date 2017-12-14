
---  
layout: post  
title: BroadcastReceiver
date: 2017-12-02 02:00:00 +0800 
categories: AS BroadcastReceiver
---  

## 广播监听网络变化
```JAVA
public class MainActivity extends AppCompatActivity {
    private IntentFilter intentFilter;
    private NetworkchangeReceiver networkChangeReceiver;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        intentFilter =new IntentFilter();                                 //创建意图过滤器
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");   //添加需要监听的广播
        networkChangeReceiver=new NetworkchangeReceiver();                //创造实例
        registerReceiver(networkChangeReceiver,intentFilter);             //注册接收器和意图过滤器
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(networkChangeReceiver);                        //销毁后动态广播一定要取消注册
    }
    class NetworkchangeReceiver extends BroadcastReceiver{
        @Override
        public void onReceive(Context context, Intent intent) {
            Toast.makeText(context,"network changes",Toast.LENGTH_SHORT).show();
        }
    }
}
```
## 开机启动静态广播
as内右键app包→new→other→Broadcast Receiver  

Exported√   允许该广播接收器接收本程序以外的广播  

Enabled√     启用该广播接收器  

使用该创建方式创建广播会自动在AndroidManifest.xml里注册  
```XML
<receiver    
android:name=".BootCompleteReceiver"    
android:enabled="true"    
android:exported="true">    
<intent-filter>                                                               //过滤器        
<action android:name="android.intent.action.BOOT_COMPLETED"/>             //接收系统的广播        
</intent-filter>                                                          //类里写接受到广播后的逻辑，例子是开机启动完毕的广播
</receiver>
```
## Intent发送自定义广播
1、创建静态广播MyBroadcastReceiver  
2、AndroidManifest.xml里过滤器里的action修改为自定义的action  
```XML
<receiver android:name=".MyBroadcastReceiver"    
android:enabled="true"    
android:exported="true">    
<intent-filter>        
<action android:name="com.example.broadcasttest.MY_BROADCAST"/>    
</intent-filter>    
</receiver>
```
3、创建一个按钮，点击事件里传递intent，intent里包含自定义的广播  
```JAVA
    button.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Intent intent=new Intent("com.example.broadcasttest.MY_BROADCAST");
            sendBroadcast(intent);
        }
    });
}
```
## 有序广播
1、广播优先级  
```XML
<intent-filter android:priority="100">      //优先级为100
    <action android:name="com.example.broadcasttest.MY_BROADCAST"/>
</intent-filter>
```
优先级高的先收到广播  
2、截断广播  
```JAVA
public class MyBroadcastReceiver extends BroadcastReceiver {    
	@Override    
	public void onReceive(Context context, Intent intent) {        
		Toast.makeText(context,"received in MyBroadcastReceiver",Toast.LENGTH_SHORT).show();        
		abortBroadcast();    //截断广播，阻止广播传到其他接收器    
		}
	}
```
## 本地广播
1、创建本地广播管理器和本地广播接收器变量  
```JAVA
private LocalBroadcastManager localBroadcastManager;
private LocalReceiver localReceiver;
```
2、创建本地广播接收器  
```JAVA
class LocalReceiver extends BroadcastReceiver{    
	@Override    
	public void onReceive(Context context, Intent intent) {        
		Toast.makeText(context,"received local broadcast",Toast.LENGTH_SHORT).show();    
		}
	}
```
3、本地广播管理器获取实例  
```JAVA
localBroadcastManager=localBroadcastManager.getInstance(this);
```
4、点击事件发送广播  
```JAVA
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Intent intent=new Intent("com.example.broadcasttest.LOCAL_BROADCAST");
        localBroadcastManager.sendBroadcast(intent);
    }
});
```
5、意图过滤器添加动作  
```JAVA
intentFilter.addAction("com.example.broadcasttest.LOCAL_BROADCAST");
```
6、本地广播管理器注册
```JAVA
localBroadcastManager.registerReceiver(localReceiver,intentFilter);
```
7、onDestroy取消注册本地广播管理器  
```JAVA
protected void onDestroy() {
    super.onDestroy();
    localBroadcastManager.unregisterReceiver(localReceiver);
}
```
**Markdown**
*Awesome*