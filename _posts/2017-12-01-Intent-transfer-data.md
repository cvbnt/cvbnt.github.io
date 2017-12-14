
---  
layout: post  
title: Intent transfer data
date: 2017-12-01 18:00:00 +0800 
categories: AS  
---  

## 发送数据


```JAVA
button.setOnClickListener(new View.OnClickListener() {    
	@Override    
	public void onClick(View v) {        
		String data="hello";        
		Intent intent=new Intent("a.b.c.myapplication.Action_START");        
		intent.addCategory("a.b.c.myapplication.MY_CATEGORY");        
		intent.putExtra("extra_data",data);        
		startActivity(intent);    
		}
	});
```
putExtra()第一个参数为键，用于接收方取值，第二个参数为要传递的数据 ，或如下写法
```JAVA
private static final String EXTRA_ANSWER_IS_TRUE="a.b.c.GeoQuiz.answer_is_true";    //定义键
public static Intent newIntent(Context packageContext,boolean answerIsTrue) { //创建newIntent方法，参数1为包名，参数2为要传输的变量值    
	Intent intent=new Intent(packageContext,CheatActivity.class);   //创建Intent    
	intent.putExtra(EXTRA_ANSWER_IS_TRUE,answerIsTrue);                 
	return intent;  
}
```

```JAVA
mCheatButton.setOnClickListener(new View.OnClickListener() {    
	@Override    public void onClick(View view) {        
		boolean answerIsTrue=mQuestionBank[mCurrentIndex].isAnswerTrue();     //赋值给要传输的变量        
		Intent intent=CheatActivity.newIntent(          
			QuizActivity.this,answerIsTrue
	);//变量作为参数传输到CheatActivity的new Intent方法中        
	startActivity(intent);    
	}
	}
	);
```

## 接收数据


```JAVA
public class FirstActivity extends AppCompatActivity {
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        setContentView(R.layout.first_layout);
        Intent intent=getIntent();
        String data=intent.getStringExtra("extra_data");
    }
}
```
getStringExtra()用于接收字符串数据,类似的还有getIntExtra(),getBooleanExtra()  
或  

```JAVA
private boolean mAnswerIsTrue;
mAnswerIsTrue=getIntent().getBooleanExtra(EXTRA_ANSWER_IS_TRUE,false);  //获取键值，第二参数为默认值
```
**Markdown**
*Awesome*
