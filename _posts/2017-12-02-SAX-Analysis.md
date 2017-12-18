
---

layout: post  

title: SAX Analysis

date: 2017-12-02 19:00:00 +0800 

categories: AS  Analysis

---

## 1、新建ContentHandler类继承DefaultHandler，并重写父类5个方法

startDocument()

开始XML解析时调用

startElement()

开始解析某个节点时调用

characters()

获取节点中的内容时调用

endElement()

完成解析某个节点时调用

endDocument()

完成整个XML解析时调用


## 

```java
public class ContentHandler extends DefaultHandler{
    private String nodeName;                         //定义节点
    private StringBuilder id;                         
    private StringBuilder name;
    private StringBuilder version;   
    @Override
    public void startDocument() throws SAXException {
        id=new StringBuilder();                      //初始化3个对象
        name=new StringBuilder();
        version=new StringBuilder();
    }
    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        nodeName=localName;                          //记录当前节点名
    }
    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        if ("id".equals(nodeName)){                   //根据当前节点名判断，将解析的内容添加到需要的StringBuilder对象中
            id.append(ch,start,length);
        }else if ("name".equals(nodeName)){
            name.append(ch,start,length);
        }else if ("version".equals(nodeName)){
            version.append(ch,start,length);
        }
    }
    @Override
    public void endElement(String uri, String localName, String qName) throws SAXException {
        if ("app".equals(localName)){
            Log.d("ContentHandler","id is "+id.toString().trim());          //结束该节点解析，目前带的is、name、version中带有回车和换行符，需要调用trim()方法
            Log.d("ContentHandler","name is "+name.toString().trim());
            Log.d("ContentHandler","version is "+version.toString().trim());
            id.setLength(0);                                        //打印完后清空所有StringBuilder的内容，防止影响下一次内容的读取
            name.setLength(0);
            version.setLength(0);
        }
    }
    @Override
    public void endDocument() throws SAXException {
        super.endDocument();
    }
}
```

## 2、MainActivity中调用SAX解析方法

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
          Request request=new Request.Builder().url("http://10.0.2.2:8888/get_data.xml").build(); 
          Response response=client.newCall(request).execute();                    
          String responseData=response.body().string();                    
          parseXMLWithSAX(responseData);                          //调用SAX解析方法                
        }catch (Exception e){                    
          e.printStackTrace();                
        }            
      }        
    }).start();    
  }    
  private void parseXMLWithSAX(String xmlData) {        
    try {            
      SAXParserFactory factory=SAXParserFactory.newInstance();      //创建一个SAXParserFactory实例factory            
      XMLReader xmlReader=factory.newSAXParser().getXMLReader();    //创建一个XMLReader对象            
      ContentHandler handler=new ContentHandler();                  //创建ContentHandler实例            
      xmlReader.setContentHandler(handler);                         //将ContentHandler实例添加到xmlReader中            
      xmlReader.parse(new InputSource(new StringReader(xmlData)));  //最后调用parse方法解析        
    }catch (Exception e){            
      e.printStackTrace();        
    }    
  }

```



**Markdown**
*Awesome*