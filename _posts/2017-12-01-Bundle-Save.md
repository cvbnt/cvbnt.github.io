
---  
layout: post  
title: Bundle Save  
date: 2017-12-01 14:00:00 +0800  
categories: AS  
---  

## 设置键值

```JAVA
private static final String KEY_INDEX="index";
```
  
## Activity内重写onSavaInstanceState方法

```JAVA
@Overridepublic void onSaveInstanceState(Bundle savedInstanceState) { //将数据保存在Bundle中的savedInstanceState      
	super.onSaveInstanceState(savedInstanceState);    
	savedInstanceState.putInt(KEY_INDEX,mCurrentIndex);               //赋值给savedInstanceState,参数为键值和变量
}
```
## onCreate()方法内检查储存的Bundle信息

```JAVA
protected void onCreate(Bundle savedInstanceState) {    
	if(savedInstanceState!=null){       
	 mCurrentIndex=savedInstanceState.getInt(KEY_INDEX,0);    
	}
```
protected void onSavedInstanceState(Bundle)会在onStop()方法调用前调用，除非用户按后退键  
## 恢复数据
```JAVA
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    if(savedInstanceState!=null){
        String temData=savedInstanceState.getString("data_key");
    }
```  
**Markdown**
*Awesome*
