---

layout: post  

title:   

date: 2017-12-02 13:00:00 +0800 

categories: AS  

---

## VidoView类控制方法

```java
SetVideoPath()                设置播放视屏的位置
start()                             开始或继续播放视屏
pause()                           暂停
resume()                         重置
seekTo()                          指定位置播放音频
isPlaying()                       判断当前MediaPlayer是否正在播放音频
getDuration()                   获取载入的音频文件的时长
```



## 1、布局

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"    android:orientation="vertical"    
              xmlns:tools="http://schemas.android.com/tools"    
              android:layout_width="match_parent"    
              android:layout_height="match_parent"    tools:context="a.b.c.playvideotest.MainActivity">    
  <LinearLayout        
                android:layout_width="match_parent"        
                android:layout_height="wrap_content"       
                >        
    <Button            
            android:id="@+id/play"            
            android:layout_weight="1"            
            android:layout_width="0dp"            
            android:layout_height="wrap_content"            
            android:text="Play" />        
    <Button            
            android:id="@+id/pause"            
            android:layout_weight="1"            
            android:layout_width="0dp"            
            android:layout_height="wrap_content"            
            android:text="Pause" />        
    <Button            
            android:id="@+id/replay"            
            android:layout_weight="1"            
            android:layout_width="0dp"            
            android:layout_height="wrap_content"            
            android:text="Replay" />    
  </LinearLayout>    
  <VideoView        
             android:id="@+id/video_View"        
             android:layout_width="match_parent"        
             android:layout_height="wrap_content" />
</LinearLayout>

```

## 逻辑

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{    
  private VideoView videoView;    
  @Override    
  protected void onCreate(Bundle savedInstanceState) {        
    super.onCreate(savedInstanceState);        
    setContentView(R.layout.activity_main);        
    videoView=(VideoView)findViewById(R.id.video_View);        
    Button play=(Button)findViewById(R.id.play);        
    Button pause=(Button)findViewById(R.id.pause);        
    Button replay=(Button)findViewById(R.id.replay);        
    play.setOnClickListener(this);        
    pause.setOnClickListener(this);        
    replay.setOnClickListener(this);        
    if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.WRITE_EXTERNAL_STORAGE)!= PackageManager.PERMISSION_GRANTED){            
      ActivityCompat.requestPermissions(MainActivity.this,new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},1);        
    }else {            
      initVideoPath();        
    }    
  }    
  private void initVideoPath() {        
    File file=new File(Environment.getExternalStorageDirectory(),"movie.mp4");     //        
    videoView.setVideoPath(file.getPath());    
  }    
  @Override    
  public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {        
    switch (requestCode){            
      case 1:                
        if (grantResults.length>0&&grantResults[0]==PackageManager.PERMISSION_GRANTED){                    
          initVideoPath();                
        }else {                    
          Toast.makeText(this,"拒绝权限无法使用本程序",Toast.LENGTH_SHORT).show();                    
          finish();                
        }                
      default:                    
        break;        
    }    
  }    
  @Override    
  public void onClick(View view) {        
    switch (view.getId()){            
      case R.id.play:                
        if (!videoView.isPlaying()){                    
          videoView.start();                
        }                
        break;            
      case R.id.pause:                
        if (videoView.isPlaying()){                    
          videoView.pause();                
        }                
        break;            
      case R.id.replay:                
        if (videoView.isPlaying()){                    
          videoView.resume();                
        }                
        break;            
      default:                
        break;        
    }    
  }    
  @Override    
  protected void onDestroy() {        
    super.onDestroy();        
    if (videoView!=null){            
      videoView.suspend();   //暂停VideoView        
    }    
  }
}

```

**Markdown**
*Awesome*
