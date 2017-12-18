
---  
layout: post  
title: Application Debug  
date: 2017-12-01 11:00:00 +0800  
categories: AS  
---  

## 为某个方法内添加日志输出异常，例如

```JAVA
private void  updateQuestion(){    
	Log.d(TAG,"Updating question text",new Exception());    
	int question=mQuestionBank[mCurrentIndex].getTextResId();    
	mQuestionTextView.setText(question);
}
```

## 设置断点

1、设置断点   
2、点击Debug运行   
3、app运行到该断点处暂停，断点设置所在行加亮显示   
4、Frames视图显示断点处调用的方法和方法所在的类,Variables显示目前所有的变量值(this变量为运行的类本身)。  
5、单击Debugger旁的继续运行按钮，模拟器去触发断点   
6、跳出当前方法，点击“单步执行跳出按钮”(Debugger右侧右上箭头)   
7、找到错误并修正，之前需停止调试（1、运行按钮下面的stop按钮。2、stop下面的X按钮）  

## 设置异常断点

1、顶栏Run->View Breakpoints弹出断点设置窗口   
2、点击+号设置新断点   
3、下拉列表选择Java Exception Breakpoints，捕捉异常类型选择RuntimeException   
4、选择RuntimeException(java.lang)，适用于绝大多数异常   
5、点击done，debug调试，跳转到错误所在行并显示断点  

## Android Lint
1、顶栏Analize->Inspect Code   
2、选择Whole project，检查整个项目的问题   
3、列出所有隐患  
**Markdown**
*Awesome*
