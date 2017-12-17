---

layout: post  

title: GSON Analysis 

date: 2017-12-02 21:00:00 +0800 

categories: AS  Analysis

---

## 1、新建依赖

```xml
compile 'com.google.code.gson:gson:2.8.1'
```


## 2、原理：

将一段JSON格式的字符串自动映射成一个对象，无需解析

例如一段JSON格式的数据如下

```json
{"name":"tom","age":"20"}
```

定义一个person类，加入name和age两个字段

```java
Gson gson=new Gson;
Person person=gson.fromJson(jsonData,Person.class);
```

如果需要解析一段JSON数组，需借助TypeToken将期望解析的数据类型传入fromJson()方法中

```java
List<Person> people=gson.fromJson(jsonData,new TypeToken<List<person>>(){}.getType());
```

## 3、新建App类，加入id、name、version三个字段

```java
public class App {    
  private String id;    
  private String name;    
  private String version;    
  public String getId(){        
    return id;    
  }    
  public void setId(String id) {        
    this.id = id;    
  }    
  public String getName() {        
    return name;    
  }    
  public String getVersion() {        
    return version;    
  }    
  public void setVersion(String version) {        
    this.version = version;    
  }
}
```

## 4、修改MainActivity中的代码

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{    
  TextView responseText;    
  @Override    
  protected void onCreate(Bundle savedInstanceState) {        
    super.onCreate(savedInstanceState);        
    setContentView(R.layout.activity_main);        
    Button sendRequest = (Button) findViewById(R.id.send_request);        
    responseText = (TextView) findViewById(R.id.response_text);        
    sendRequest.setOnClickListener(this);    
  }    
  @Override    
  public void onClick(View v){            
    if (v.getId() == R.id.send_request) {                
      sendRequestWithOkhttp();            
    }        
  }    
  private void sendRequestWithOkhttp() {        
    new Thread(new Runnable() {            
      @Override            
      public void run() {                
        try{                    
          OkHttpClient client=new OkHttpClient();                    
          Request request=new Request.Builder().url("http://10.0.2.2:8888/get_data.json").build();                    
          Response response=client.newCall(request).execute();                    
          String responseData=response.body().string();                    
          parseJSONWithGSON(responseData);                
        }catch (Exception e){                    
          e.printStackTrace();                
        }            
      }        
    }).start();    
  }    
  private void parseJSONWithGSON(String jsonData) {        
    Gson gson=new Gson();        
    List<App> appList=gson.fromJson(jsonData,new TypeToken<List<App>>(){}.getType());        
    for (App app:appList){            
      Log.d("MainActivity","id is "+app.getId());            
      Log.d("MainActivity","name is "+app.getName());            
      Log.d("MainActivity","version is "+app.getVersion());        
    }    
  }
```



Markdown**
*Awesome*