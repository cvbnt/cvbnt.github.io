---

layout: post  

title: AsyncTask

date: 2017-12-03 01:00:00 +0800 

categories: AS  

---

## 机制

对Thread和Handler进行了封装，方便使用

AsyncTask是个抽象类，必须子类继承它，3个参数

Params：执行AsyncTask传入的参数，在后台任务中使用

Progress:需要在界面上显示当前进度，使用指定泛型作为进度单位

Result:任务执行完毕后，需要对结果进行放回，使用指定泛型作为返回值类型

例如

class DownloadTask extends AsyncTask<Void,Integer,Boolean>{...}

void表示不需要传入参数给后台任务，Integer表示使用整形数据来作为进度显示单位，Boolean表示使用布尔型数据来反馈结果

需要重写的方法

1、onPreExecute()

后台任务开始执行前调用，如显示进度对话框

2、doInBackground(Parms....)

该方法所有代码在子线程中允许，在这里处理所有耗时任务，任务完成用return返回任务执行结果，如果AsyncTask第三个参数是Void，则不返回任务执行结果，该方法内无法执行UI操作，如果要更新UI，则需调用publishProgress(Progress...)完成

3、onProgressUpdate(Progress...)

  后台任务调用publishProgress(Progress...)后，马上会调用onProgressUpdate(Progress...)方法，Progress是后台任务传递过来的，该方法可以进行UI操作，利用参数数值更新

  4、onPostExecute(Result)

   后台任务执行完毕后return语句返回时，该方法很快被调用，返回的数据作为参数传递到该方法中，可以利用返回的数据进行UI操作，

## 例子：定义一个下载任务

```java
public class DownloadTASK extends AsyncTask <Void,Integer,Boolean>{    
  @Override    
  protected void onPreExecute() {        
    progressDialog.show();                                  //1、显示进度对话框    
  }    
  @Override     
  protected Boolean doInBackground(Void... params) {          //2、执行具体下载任务，子线程运行        
    try {            
      while (true) {                
        int downloadParcent = doWnload();                
        publishProgress(downloadParcent);               //3、得到当前下载进度后，调用publishProgress将下载进度传入                
        if (downloadParcent >= 100) {                    
          break;                
        }            
      }        
    } catch (Exception e) {            
      return false;                                                
    }        
    return true;                                             //5、下载完成后result为true    
  }    
  @Override    
  protected void onProgressUpdate(Integer... values) {          //4、第三步完成后，马上调用该方法，进行UI操作        
    progressDialog.setMessage("Downloaded "+ values[0]+ "%");    
  }    
  @Override    
  protected void onPostExecute(Boolean result) {        
    progressDialog.dismiss();        
    if (result){                                                                //6、收尾工作，下载完成后，弹出提示            
      Toast.makeText(Context,"Download succeeded",Toast.LENGTH_SHORT).show();        
    }else {            
      Toast.makeText(Context,"Download failed",Toast.LENGTH_SHORT).show();        
    }    
  }
}
```

**Markdown**
*Awesome*