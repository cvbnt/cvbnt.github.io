
---  
layout: post  
title: Add Button  
date: 2017-12-01 12:00:00 +0800  
categories: AS  
---  

## 创建按钮对象

onCreate方法中：  
```JAVA
Button button=(Button)findViewById(R.id.button);
```

## 添加点击事件

onCreate方法中
```JAVA
button.setOnClickListener(
	new View.OnClickListener() {    
		@Override    
		public void onClick(View v) {    
			}
		}
	);
```

**Markdown**
*Awesome*
