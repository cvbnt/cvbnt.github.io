
---

layout: post  

title: Network Programming Skills

date: 2017-12-02 22:00:00 +0800 

categories: AS  Skills

---

## 1、当需要多次HTTP请求时，自定义静态方法并调用即可

```java
public class HttpUtil {    
  public static String sendHttpRequest(String adress) {        
    HttpURLConnection connection = null;        
    try {           
      URL url = new URL(adress);            
      connection = (HttpURLConnection) url.openConnection();            
      connection.setRequestMethod("GET");            
      connection.setConnectTimeout(8000);            
      connection.setReadTimeout(8000);            
      connection.setDoInput(true);            
      connection.setDoOutput(true);            
      InputStream in = connection.getInputStream();            
      BufferedReader reader = new BufferedReader(new InputStreamReader(in));            
      StringBuilder response = new StringBuilder();           
      String line;            
      while ((line = reader.readLine()) != null) {                
        response.append(line);            
      }            
      return response.toString();        
    } catch (Exception e) {            
      e.printStackTrace();            
      return e.getMessage();        
    } finally {            
      if (connection != null) {                
        connection.disconnect();            
      }        
    }    
  }
}
```

当需要发起HTTP请求时这样写

```java
String adress=http://www.baidu.com;
String response=HttpUtil.sendHttpRequest(adress);
```

## 2、网络请求属于耗时操作，有可能在调用sendHttpRequset()方法时使主线程阻塞，需要在sendHttpRequset()内开启子线程，但服务器数据还没返回时sendHttpRequest()方法已经执行结束。解决方法：回调机制

新建接口

```java
public interface HttpCallbackListener{    
  void onFinish(String response);         //服务器成功响应请求时调用    
  void onError(Exception e);              //网络操作出错时调用
}
```

修改HttpUtil代码

```java
public class HttpUtil {
    public static void sendHttpRequest(final String adress, final HttpCallbackListener listener) {  //添加一个HttpCallbackListener参数
        new Thread(new Runnable() {                          //开启子线程，并且子线程是无法通过return语句来返回数据，所以将数据传入onFinish()方法中
            @Override
            public void run() {
                HttpURLConnection connection = null;
                try {
                    URL url = new URL(adress);
                    connection = (HttpURLConnection) url.openConnection();
                    connection.setRequestMethod("GET");
                    connection.setConnectTimeout(8000);
                    connection.setReadTimeout(8000);
                    connection.setDoInput(true);
                    connection.setDoOutput(true);
                    InputStream in = connection.getInputStream();
                    BufferedReader reader = new BufferedReader(new InputStreamReader(in));
                    StringBuilder response = new StringBuilder();
                    String line;
                    while ((line = reader.readLine()) != null) {
                        response.append(line);
                    }
                    if (listener!=null){
                        listener.onFinish(response.toString());
                    }
                } catch (Exception e) {
                    if (listener!=null){
                        listener.onError(e);                         //出现异常则将异常传入onError()方法中
                    }
                } finally {
                    if (connection != null) {
                        connection.disconnect();
                    }
                }
            }
        }).start();
    }
}
```

最终调用时

```java
HttpUtil.sendHttpRequest(adress, new HttpCallbackListener() {    
  @Override    
  public void onFinish(String response) {        //根据返回内容执行具体逻辑    
  }    
  @Override    
  public void onError(Exception e) {        //对异常情况进行处理    
  }
});
```

## 3、使用OkHttp更简单   

在HttpUtil中加入sendOkHttpRequest()方法

```java
public static void sendOkHttpRequset(String adress,okhttp3.Callback callback){   //okHttp3.Callback参数类似于自己编写的HttpCallbackListener    
  OkHttpClient client=new OkHttpClient();    
  Request request=new Request().Builder()            
    .url(address)            
    .build();    
  client.newCall(request).enqueue(callback);
}
```

 最终调用时

```java
HttpUtil.sendOkHttpRequset("http://www.baidu.com",new HttpCallbackListener(){             
  @Override        
  public void onFinish(String response) {                    
  }        
  @Override        
  public void onError(Exception e) {        
  }        
  public void onResponse(Call call, Response response)throws IOException{            //得到服务器返回的具体内容            
    String responseData=response.body().string();        
  }        
  public void onFailure(Call call,IOException e){            //对异常情况处理        
  }    
});
}
```



**Markdown**
*Awesome*