
---  
layout: post  
title: SharedPreferences
date: 2017-12-02 05:00:00 +0800 
categories: AS Data
---  

## 流程
1、获得SharedPreferences对象  

①getSharedPreferences()  

参数1，文件名称  

参数2，操作模式，默认MODE_PRIVATE  

②getPreferences()  
 
只接收操作模式作为参数  

③getDefaultSharedPreferences()  

只接受一个context参数，自动使用包名前缀命名SharedPreferences文件  

2、存储数据  

①调用对象的edit()方法，获取SharedPreferences.Editor对象  

②向SharedPreferences.Editor传递数据，添加布尔型数据putBoolean()  

添加字符串putString()  

③调用apply()提交数据。  
 
## 数据传到文件
```JAVA
savaData.setOnClickListener(new View.OnClickListener() {    
	@Override    
	public void onClick(View v) {        
		SharedPreferences.Editor editor=getSharedPreferences("data",MODE_PRIVATE).edit();  //创建SharedPreferences.Editor对象，参数为文件名和模式        
		editor.putString("name","tom");    //输入字符串        
		editor.putInt("age",28);           //输入整形数据        
		editor.putBoolean("married",false);//输入布尔型变量        
		editor.apply();                    //提交
        #editor.clear();                   //清除    
        }
       }
     );
```
## 读取文件数据
```JAVA
restoreData.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        SharedPreferences pref=getSharedPreferences("data",MODE_PRIVATE); //创建SharedPreferences对象，读取的文件名为data，模式
        String name=pref.getString("name","");              //获取姓名的字符串 ，后为取不到情况下的默认值  
        int age=pref.getInt("age",0);                       //获取年龄的数据
        boolean married=pref.getBoolean("married",false);   //获取布尔数据
        Log.d("MainActivity","name is "+ name);             //打印
        Log.d("MainActivity","age is "+ age); 
        Log.d("MainActivity","married is "+ married);
    }
});
```
**Markdown**
*Awesome*