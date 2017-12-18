
---

layout: post  

title: HttpURLConnection

date: 2017-12-02 15:00:00 +0800 

categories: AS  Http Web

---

## 基本流程

1、创建Url对象

```java
URL url = new URL("http://www.baidu.com");
```

获取到HttpURLConnection实例，并调用openConnection()方法  

```java
Connection = (HttpURLConnection) url.openConnection();
```

设置HTTP请求方法，GET或POST，获取数据或提交数据

```java
connection.setRequestMethod("GET
```

设置连接超时、读取超时的毫秒数

```java
connection.setConnectTimeout(8000);
connection.setReadTimeout(8000);
```

2、获取服务器返回的输入流

```java
InputStream in=connection.getInputStream();
```

3、关闭HTTP连接

```java
connection.disconnect();
```

## 例子

1、布局

```xml
<LinearLayout    
              xmlns:android="http://schemas.android.com/apk/res/android"    
              xmlns:tools="http://schemas.android.com/tools"    
              android:orientation="vertical"    
              android:layout_width="match_parent"    
              android:layout_height="match_parent"    
              tools:context="a.b.c.networktest.MainActivity">    
  <Button        
          android:id="@+id/send_request"        
          android:layout_width="match_parent"        
          android:layout_height="wrap_content"        
          android:text="Send Request" />    
  <ScrollView        
              android:layout_width="match_parent"        
              android:layout_height="match_parent">        
    <TextView            
              android:id="@+id/response_text"            
              android:layout_width="match_parent"            
              android:layout_height="wrap_content" />    
  </ScrollView></LinearLayout>

```

效果：点击按钮获取到服务器的输入流并显示在TextView中

```Java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    TextView responseText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button sendRequest=(Button)findViewById(R.id.send_request);
        responseText=(TextView)findViewById(R.id.response_text);
        sendRequest.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        if (v.getId()==R.id.send_request){
            sendRequestWithHttpURLConnection();
        }
    }

    private void sendRequestWithHttpURLConnection() {                    
        new Thread(new Runnable() {                                              //开启子线程
            @Override
            public void run() {
                HttpURLConnection connection = null;
                BufferedReader reader = null;
                try {
                    URL url = new URL("http://www.baidu.com");
                    connection = (HttpURLConnection) url.openConnection();
                    connection.setRequestMethod("GET");
                    connection.setConnectTimeout(8000);
                    connection.setReadTimeout(8000);
                    InputStream in = connection.getInputStream();
                    reader = new BufferedReader(new InputStreamReader(in));      //利用BufferedReader对服务器返回的数据流进行读取
                    StringBuilder response = new StringBuilder();                
                    String line;
                    while ((line = reader.readLine()) != null) {                 //当reader读取一行数据不为空时，赋值给line，response对象附加上line字符串
                        response.append(line);
                    }
                    showResponse(response.toString());                            //调用showResponse传入response数据
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    if (reader != null) {
                        try {
                            reader.close();                                      
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                    if (connection != null) {
                        connection.disconnect();
                    }
                }
            }
        }).start();

    }

    private void showResponse(final String response) {
        runOnUiThread(new Runnable() {                                        //子线程无法进行UI操作，runOnUiThread()方法将线程切换到主线程
            @Override
            public void run() {
                responseText.setText(response);                               //显示数据
            }
        });
    }


}
```

权限：

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```




##



**Markdown**
*Awesome*