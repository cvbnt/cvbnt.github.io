
---

layout: post  

title: Multithreading 

date: 2017-12-02 23:00:00 +0800 

categories: AS  

---

## 1、创建线程

```java
class MyThread extends Thread{    
  @Override    
  public void run() {            
  }
}
```

启动线程

```java
new MyThread().start();
```

## 2、Runnable接口定义线程

```java
class MyThread implements Runnable{    
  @Override    
  public void run() {            
  }
}
```

启动线程

```java
Thread myThread=new MyThread();  
new Thread(myThread).start();
```


##3、匿名类实现

```java
 new Thread(new Runnable() {        
 @Override        
 public void run() {                    
 }    
 }).start();
 }
```

**Markdown**
*Awesome*