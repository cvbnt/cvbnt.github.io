---

layout: post  

title: Camera And Image

date: 2017-12-02 11:00:00 +0800 

categories: AS  Permission Camera Image

---

## 布局

```xml
<?xml version="1.0" encoding="utf-8"?><LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"    
android:orientation="vertical"    
xmlns:tools="http://schemas.android.com/tools"    
android:layout_width="match_parent"   
android:layout_height="match_parent"    
tools:context="a.b.c.cameraalbumtest.MainActivity">    
<Button        
    android:id="@+id/take_photo"        
    android:layout_width="match_parent"        
    android:layout_height="wrap_content"        
    android:text="Take Photo" />
<ImageView   
    android:id="@+id/picture"    
    android:layout_width="wrap_content"    
    android:layout_height="wrap_content"                                                               android:layout_gravity="center_horizontal"/>
</LinearLayout>

```

## 逻辑

```java
public class MainActivity extends AppCompatActivity {    
  public final static int TAKE_PHOTO=1;    
  private ImageView picture;    
  private Uri imageUri;    
  @Override    
  protected void onCreate(Bundle savedInstanceState) {        
    super.onCreate(savedInstanceState);        
    setContentView(R.layout.activity_main);        
    Button takePhoto=(Button)findViewById(R.id.take_photo);        
    picture =(ImageView)findViewById(R.id.picture) ;        
    takePhoto.setOnClickListener(new View.OnClickListener() {            
      @Override            
      public void onClick(View view) {                
        File outputImage = new File(getExternalCacheDir(), "output_image.jpg");  //创建File对象，用于存放拍下的图片，
                 
        //getExternalCacheDir()方法得到图片的应用关联缓存目录/sdcard/Android/data/<package name>/cache                
        try {                    
          if (outputImage.exists()) {    //如果图片存在则删除                        outputImage.delete();                          
          }                    
          outputImage.createNewFile();   //不存在则创建                
        } catch (IOException e) {                    
          e.printStackTrace();                
        }                
        if (Build.VERSION.SDK_INT>=24){                    
          imageUri=FileProvider.getUriForFile(MainActivity.this,"a.b.c.cameraalbumtest.provider",outputImage);
//如果版本大于Android7.0,调用Uri的fromFile()方法将File对象转换成Uri对象，该Uri对象标识着这张本地图片的真实路径，getUriForFile()方法，参数1是context，参数2唯一字符串，参数3是File对象                
        }else {                    
          imageUri=Uri.fromFile(outputImage);//Android7.0开始直接使用本地真实路径被认为不安全，需要转换                
        }                
        Intent intent=new Intent("android.media.action.IMAGE_CAPTURE"); //意图是拍照                
        intent.putExtra(MediaStore.EXTRA_OUTPUT,imageUri); //putExtra()方法指定图片输出地址，填入Uri对象                
        startActivityForResult(intent,TAKE_PHOTO);         //启动活动，并且会有结果返回到onActivityResult()方法中            
      }        
    });    
  }    
  @Override    
  protected void onActivityResult(int requestCode, int resultCode, Intent data) {        
    switch (requestCode){            
      case TAKE_PHOTO:                
        if (resultCode==RESULT_OK){   //启动活动成功                    
          try {                        
            Bitmap bitmap= BitmapFactory.decodeStream(getContentResolver().openInputStream(imageUri));//将照片解析成Bitmap对象                        
            picture.setImageBitmap(bitmap);   //并设置成在imageview中显示                    
          }catch (IOException e){                        
            e.printStackTrace();                    
          }                
        }                
        break;            
      default:                
        break;        
    }    
  }
}

```

2、 在Manifest.xml文件中注册内容提供器

```xml
<provider    
          android:authorities="a.b.c.cameraalbumtest.provider"//和MainActivity中的FileProvider.getUriForFile()方法的第二个参数一致   android:name="android.support.v4.content.FileProvider"    //固定    
android:exported="false"    
android:grantUriPermissions="true">    
  <meta-data        
 android:name="android.support.FILE_PROVIDER_PATHS"    //指定Uri的共享路径                      android:resource="@xml/file_paths"/>                  //引用file_paths.xml文件    
</provider>

```

3、新建/res/xml/file_paths.xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>    
<paths xmlns:android="http://schemas.android.com/apk/res/android">    
  <external-path name="my_images" path=""/>                        //name随便，path为空代表整个SD卡共享</paths>

```

4、Android4.4版本前需要声明权限

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```




## 



**Markdown**
*Awesome*