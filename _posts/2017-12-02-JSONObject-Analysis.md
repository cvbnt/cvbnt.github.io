---

layout: post  

title: JSONObject Analysis

date: 2017-12-02 20:00:00 +0800 

categories: AS  Analysis

---

## 1、Apache24/htdocs创建文件get_data.json

```json
[{"id":"5","version":"5.5","name":"Clash of Clans"},

{"id":"6","version":"7.0","name":"Boom Beach"},

{"id":"7","version":"3.5","name":"Clash Royale"}]

```

## 代码

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
                    parseJSONWithJSONObject(responseData);                   //新建方法解析json语句
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        }).start();
    }
    private void parseJSONWithJSONObject(String jsonData) {
        try{
            JSONArray jsonArray=new JSONArray(jsonData);                     //将服务器返回的数据传入JSONArray对象中，
            for (int i=0;i<jsonArray.length();i++){                          //遍历循环JSONArray,从中取出的每一个元素都是一个JSONObject对象，每个对象包含id,name,version
                JSONObject jsonObject=jsonArray.getJSONObject(i++);{          
                    String id=jsonObject.getString("id");                     //调用getString方法取出数据
                    String name=jsonObject.getString("name");
                    String version=jsonObject.getString("version");
                    Log.d("MainActivity","id is "+id);
                    Log.d("MainActivity","name is "+name);
                    Log.d("MainActivity","version is "+version);
                }
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
```



**Markdown**
*Awesome*