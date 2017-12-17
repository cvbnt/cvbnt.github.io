---

layout: post  

title: Play Music

date: 2017-12-02 12:00:00 +0800 

categories: AS  Music Permission

---

## MediaPlayer类控制方法

```java
SetDataSource()             设置播放音频的位置

prepare()                        播放前完成准备工作

start()                            开始或继续播放音频

pause()                          暂停

reset()                            重置

seekTo()                         指定位置播放音频

stop()                             停止播放，调用该方法后MediaPlayer对象无法再播放音频

release()                         释放掉与MediaPlayer对象相关的资源

isPlaying()                       判断当前MediaPlayer是否正在播放音频

getDuration()                   获取载入的音频文件的时长

```



## 设计布局

播放按钮，暂停按钮，播放时重置按钮

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"    android:orientation="vertical"    
              xmlns:tools="http://schemas.android.com/tools"    
              android:layout_width="match_parent"    
              android:layout_height="match_parent"    tools:context="a.b.c.playaudiotest.MainActivity">    
  <Button        
          android:id="@+id/play"        
          android:layout_width="match_parent"        
          android:layout_height="wrap_content"        
          android:text="Play" />    
  <Button        
          android:id="@+id/pause"        
          android:layout_width="match_parent"        
          android:layout_height="wrap_content"        
          android:text="Pause" />    
  <Button        
          android:id="@+id/stop"        
          android:layout_width="match_parent"        
          android:layout_height="wrap_content"        
          android:text="Stop" />
</LinearLayout>
```

## 逻辑

```java
public class MainActivity extends AppCompatActivity  implements View.OnClickListener{    
  private MediaPlayer mediaPlayer=new MediaPlayer();          //创建MediaPlayer对象，利用对象实现播放暂停重置功能    
  @Override    
  protected void onCreate(Bundle savedInstanceState) {        
    super.onCreate(savedInstanceState);        
    setContentView(R.layout.activity_main);        
    Button play=(Button)findViewById(R.id.play);        
    Button pause=(Button)findViewById(R.id.pause);        
    Button stop=(Button)findViewById(R.id.stop);        
    play.setOnClickListener(this);        
    pause.setOnClickListener(this);        
    stop.setOnClickListener(this);        
    if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.WRITE_EXTERNAL_STORAGE)!= PackageManager.PERMISSION_GRANTED){            
      ActivityCompat.requestPermissions(MainActivity.this,new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},1);        
    }else {            
      initMediaPlayer();   //得到权限后调用initMediaPlayer()方法        
    }    
  }    
  private void initMediaPlayer() {        
    try{            
      File file=new File(Environment.getExternalStorageDirectory(),"music.mp3");  //创建File对象指定手机根目录下的music.mp3文件            
      mediaPlayer.setDataSource(file.getPath());                                  //设置mediaPlayer得到music.mp3的路径            
      mediaPlayer.prepare();                                                      //让mediaPlayer进入到准备状态        
    }catch (Exception e){            
      e.printStackTrace();        
    }    
  }    
  @Override    
  public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {        
    switch (requestCode){            
      case 1:                
        if (grantResults.length>0&&grantResults[0]==PackageManager.PERMISSION_GRANTED){                    initMediaPlayer();                
                                                                                      }else {                    
          Toast.makeText(this,"拒绝权限将无法使用程序",Toast.LENGTH_SHORT).show();                    
          finish();                
        }                
        break;            
      default:        
    }    
  }    
  @Override    
  public void onClick(View view) {        
    switch (view.getId()){            
      case R.id.play:                
        if (!mediaPlayer.isPlaying()){                    
          mediaPlayer.start();        //播放                
        }                
        break;            
      case R.id.pause:                
        if (mediaPlayer.isPlaying()){                    
          mediaPlayer.pause();        //暂停                
        }                
        break;            
      case R.id.stop:                
        if (mediaPlayer.isPlaying()){                    
          mediaPlayer.reset();        //重置            
        }            
        break;            
      default:                
        break;        
    }    
  }    
  @Override    
  protected void onDestroy() {        
    super.onDestroy();        
    if (mediaPlayer!=null){                
      mediaPlayer.stop();                
      mediaPlayer.release(); //APP处于onDestroy时释放MediaPlayer的资源        
    }    
  }
}
```

3、声明权限

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

**Markdown**
*Awesome*