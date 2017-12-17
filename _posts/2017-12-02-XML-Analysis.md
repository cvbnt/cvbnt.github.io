---

layout: post  

title: XML Analysis 

date: 2017-12-02 18:00:00 +0800 

categories: AS  

---

## 1、Apache目录下，htdocs文件夹内创建get_data.xml，用于解析

```xml
<apps>
 <app>
  <id>1</id>
  <name>Google Maps</name>
  <version>1.0</version>
 </app>
 <app>
  <id>2</id>
  <name>Chrome</name>
  <version>2.1</version>
 </app>
 <app>
  <id>3</id>
  <name>Google Play</name>
  <version>2.3</version>
 </app>
</apps>
```



## 解析代码

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
          Request request=new Request.Builder().url("http://10.0.2.2:8888/get_data.xml").build();   //10.0.2.2代表本机，8888为自己设定的端口                    
          Response response=client.newCall(request).execute();                    
          String responseData=response.body().string();                    
          parseXMLWithPull(responseData);  //按下面的parseXMLWithPull()方法解析                
        }catch (Exception e){                    
          e.printStackTrace();                
        }            
      }        
    }).start();    
  }    
  private void parseXMLWithPull(String xmlData) {                                    
    try{            
      XmlPullParserFactory factory=XmlPullParserFactory.newInstance();  //创建一个XmlPullParserFactory实例factory            
      XmlPullParser xmlPullParser=factory.newPullParser(); //借助factory得到XmlPullParser对象 xmlPullParser            
      xmlPullParser.setInput(new StringReader(xmlData)); //调用XmlPullParser的setInput()方法解析XML数据            
      int eventType=xmlPullParser.getEventType();                                    //getEventType()方法可以得到当前解析的事件            
      String id="";            
      String name="";            
      String version="";            
      while (eventType!=XmlPullParser.END_DOCUMENT){  //如果当前解析事件不等于XmlPullParser.END_DOCUMENT，不断解析                
        String nodeName=xmlPullParser.getName(); //getNmae()方法可以得到当前节点名字                
        switch (eventType){                    
          case XmlPullParser.START_TAG: {                        
            if ("id".equals(nodeName)) {      //如果节点名等于id，name或version，则调用nextInt方法获取节点具体内容                            
              id = xmlPullParser.nextText();                        
            } else if ("name".equals(nodeName)) {                            
              name = xmlPullParser.nextText();                        
            } else if ("version".equals(nodeName)) {                            
              version = xmlPullParser.nextText();                        
            }                        
            break;                    
          }                        
          case XmlPullParser.END_TAG:{                                      //解析完后打印出来                            
            if ("app".equals(nodeName)){                                
              Log.d("MainActivity","id is "+id);                                
              Log.d("MainActivity","name is "+name);                                
              Log.d("MainActivity","version is "+version);                            
            }                            
            break;                        
          }                        
          default:                            
            break;                
        }                
        eventType=xmlPullParser.next();            
      }        
    }catch (Exception e) {            
      e.printStackTrace();        
    }        
  }

```

**Markdown**
*Awesome*