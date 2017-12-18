
---  
layout: post  
title: AlertDialog
date: 2017-12-02 03:00:00 +0800 
categories: AS  Dialog
---  

## 1、创建对话框实例
```JAVA
AlertDialog.Builder builder=new AlertDialog.Builder(context);
```
builder是对话框实例
## 2、设置标题
```JAVA
builder.setTitle("warning");
```
## 3、设置文本
```JAVA
builder.setMessage("message");
```
## 4、设置为不可用返回键撤销对话框
```JAVA
builder.setCancelable(false);
```
## 5、设置活动按钮，按钮含有监听器  
```JAVA
builder.setPositiveButton("ok", new DialogInterface.OnClickListener() {    
	@Override    
	public void onClick(DialogInterface dialog, int which) {    
		}
     });
```
## 6、最后一步，展示对话框 
**Markdown**
*Awesome*