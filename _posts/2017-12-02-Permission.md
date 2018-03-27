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
权限声明最好写在<application>标签面前

## 4、权限列表

![permission_list](https://developer.android.com/reference/android/Manifest.permission.html)

# 另一种写法

## 1、声明权限

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

## 2、添加权限常量

```java
    private static final String[] PERMISSIONS=new String[]{
            Manifest.permission.READ_EXTERNAL_STORAGE,
            Manifest.permission.WRITE_EXTERNAL_STORAGE,
    };
```

*读取和写入权限为危险权限，所有危险的 Android 系统权限都属于权限组，

- 如果应用请求其清单中列出的危险权限，而应用在同一权限组中已有另一项危险权限，则系统会立即授予该权限，而无需与用户进行任何交互。例如，如果某应用已经请求并且被授予了 `READ_CONTACTS` 权限，然后它又请求 `WRITE_CONTACTS`，系统将立即授予该权限。

任何权限都可属于一个权限组，包括正常权限和应用定义的权限。但权限组仅当权限危险时才影响用户体验。可以忽略正常权限的权限组。

如果设备运行的是 Android 5.1（API 级别 22）或更低版本，并且应用的 [`targetSdkVersion`](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html?hl=zh-cn#target) 是 22 或更低版本，则系统会在安装时要求用户授予权限。再次强调，系统只告诉用户应用需要的权限*组*，而不告知具体权限。

![normal-dangerous](https://developer.android.com/guide/topics/security/permissions.html?hl=zh-cn#normal-dangerous)

## 3、权限检查

```java
private boolean checkPermissions(){
    int result=ContextCompat.checkSelfPermission(Login.this,STORAGE_PERMISSIONS[0]);
    return result==PackageManager.PERMISSION_GRANTED;
}
```

可以避免版本判断和兼容问题

## 4、添加权限检查

```java
private static final int REQUEST_STORAGE_PERMISSIONS=0;
```

```java
if (checkPermissions()){
    Intent intent = new Intent(Login.this, MainActivity.class);
    startActivity(intent);
}else {
    requestPermissions(STORAGE_PERMISSIONS,REQUEST_STORAGE_PERMISSIONS);
}
```

如果权限检查不通过，调用requestPermissions(...)方法请求授权，调用后，系统会弹出系统权限授权对话框要求用户反馈。用户点击允许或拒绝后，继续调用onRequestPermissionsResult(...)响应方法。

## 5、针对授权结果做出反馈

```java
@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    switch (requestCode){
        case REQUEST_STORAGE_PERMISSIONS:
            if(checkPermissions()){
                Intent intent = new Intent(Login.this, MainActivity.class);
                startActivity(intent);
            }
            default:
                super.onRequestPermissionsResult(requestCode,permissions,grantResults);
    }
```

*Markdown*
*Awesome*