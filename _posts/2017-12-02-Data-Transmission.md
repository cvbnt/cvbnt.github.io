
---  
layout: post  
title: Data Transmission 
date: 2017-12-02 04:00:00 +0800 
categories: AS Data
---  

## 数据传输到文件
1、layout有一个EditText,id为edit  

2、实例化EditText  
```JAVA
edit=(EditText)findViewById(R.id.edit);
```
3、活动销毁前获取EditText内的字符串，变量inputText保存该字符串，调用自定义的save方法保存inputText  
```JAVA
protected void onDestroy() {    
	super.onDestroy();    
	String inputText=edit.getText().toString();    
	save(inputText);
}
```
4、自定义save方法
```JAVA
public void save(String inputText){    
	FileOutputStream out=null;    //创建FileOutStream对象out    
	BufferedWriter writer=null;   //创建BufferedWriter对象writer    
	try{        
		out=openFileOutput("data", Context.MODE_PRIVATE);       //方法的第一参数为文件名称，第二参数为模式，例子为默认模式        
		writer=new BufferedWriter(new OutputStreamWriter(out)); //缓冲流对象指定out的属性        
		writer.write(inputText);                                //往文件data通过默认模式写入inputText    
		}catch (IOException e){        
			e.printStackTrace();                                    //以下为异常处理方式    
			}finally {            
				try{                
					if(writer !=null){                    
						writer.close();                
						}            
						}catch (IOException e){                
							e.printStackTrace();            
							}    
							}
						}
```
## 文件传输到Activity
1、自定义load()方法  
```JAVA
public String load() {
    FileInputStream in=null;                    //创建FileInputStream对象in
    BufferedReader reader=null;                 //创建缓冲阅读器对象reader
    StringBuilder content=new StringBuilder();  //创建StringBuilder对象content
    try{
        in =openFileInput("data");                              //in对象为调用openFileInput打开data文件
        reader=new BufferedReader(new InputStreamReader(in));   //reader对象绑定in对象
        String line ="";                                        //字符串line为空
        while ((line=reader.readLine())!=null){                 //while循环，检测阅读器阅读data文件字符如果不为null，则content附加上字符串line
            content.append(line);                         
        }
    }catch (IOException e){
        e.printStackTrace();
    }finally {
        if(reader !=null){
            try{
                reader.close();
            }catch (IOException e){
                e.printStackTrace();
            }
        }
        return content.toString();                         //最后一步，将content字符串值返回
    }
}
```
2、活动的onCreate方法中，传递data中的值到EditText中
```JAVA
protected void onCreate(Bundle savedInstanceState) {    
	super.onCreate(savedInstanceState);    
	setContentView(R.layout.activity_main);    
	edit=(EditText)findViewById(R.id.edit);    
	String inputText=load();                            //load()方法返回date文件内的值，将值赋予给inputText    
	if (!TextUtils.isEmpty(inputText)){                 //当TextUtils.isEmpty（inputText）内的值为null或空时，会返回true        
		edit.setText(inputText);                        //edit内填充inputText        
		edit.setSelection(inputText.length());          //edit将光标移动inputText字符串长度（就是末尾），方便输入        
		Toast.makeText(this,"Restoring succeeded",Toast.LENGTH_SHORT).show();   //弹出还原成功的提示    
		}
	}
```
**Markdown**
*Awesome*