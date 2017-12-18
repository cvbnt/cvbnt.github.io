
---  
layout: post  

title: Permission  

date: 2017-12-02 08:00:00 +0800  

categories: AS Permission  

---  

## 1、添加要打电话的按钮,id为make_call


## 代码
```JAVA
public class MainActivity extends AppCompatActivity {    
	@Override    
	protected void onCreate(Bundle savedInstanceState) {        
		super.onCreate(savedInstanceState);        
		setContentView(R.layout.activity_main);        
		Button makeCall=(Button)findViewById(R.id.make_call);        
		makeCall.setOnClickListener(new View.OnClickListener() {            
			@Override            
			public void onClick(View v) {                
				if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.CALL_PHONE)!= PackageManager.PERMISSION_GRANTED)//ContextCompat.checkSelfPermission()方法接收context和具体的权限名，返回值和PackageManager.PERMISSION_GRANTED比较，相等则授权，不等则没有授权                      
				{                    
					ActivityCompat.requestPermissions(MainActivity.this,new String[]{ Manifest.permission.CALL_PHONE},1);//没有授权，则调用ActivityCompat.requestPermissions()方法向用户申请授权，第一个参数是Activity实例，第二个参数是String数组，权限名放在数组内，第三个参数为请求码，传入1                
					}else {                    
						call();  //调用自定义的打电话方法                
					}            
				}        
			});    
		}    
		private void call(){        
			try {            
				Intent intent = new Intent(Intent.ACTION_CALL);            
				intent.setData(Uri.parse("tel:10086"));   //拨打10086            
				startActivity(intent);        
			}        
			catch (SecurityException e){            
				e.printStackTrace();        
			}    
		}       //权限申请的对话框无论同意或拒绝，都会回调onRequestPermissionsResult()方法，授权结果封装在grantResults参数中，    
		@Override    
		public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {        
			switch (requestCode){            
				case 1:    //请求码为1，grantResults值不为空，则已授权                
				if(grantResults.length>0&&grantResults[0]==PackageManager.PERMISSION_GRANTED){                                        
					  call();                
					}else {                    
						Toast.makeText(this,"You denied the permission",Toast.LENGTH_SHORT).show();                
					}                
					break;            
				default:        
			}    
		}
	}
```
## 3、AndroidManifest.xml添加权限
```XML
           <uses-permission android:name="android.permission.CALL_PHONE"/>
```
**Markdown**
*Awesome*