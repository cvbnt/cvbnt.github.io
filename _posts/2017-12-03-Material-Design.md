---

layout: post  

title: Material Design

date: 2017-12-03 02:00:00 +0800 

categories: AS  Design

---

## 修改布局

```xml
<android.support.v4.widget.DrawerLayout                               //最外层控件使用DrawerLayout，包含两个直接子控件    
               xmlns:android="http://schemas.android.com/apk/res/android"    
               xmlns:app="http://schemas.android.com/apk/res-auto"    
               android:id="@+id/drawer_layout"    
               android:layout_width="match_parent"    
               android:layout_height="match_parent">
  <FrameLayout                                                          //第一个子控件    
               xmlns:android="http://schemas.android.com/apk/res/android"    
               xmlns:tools="http://schemas.android.com/tools"    
               xmlns:app="http://schemas.android.com/apk/res-auto"    
               android:layout_width="match_parent"    
               android:layout_height="match_parent"    
               tools:context="a.b.c.materialtest.MainActivity">
  <android.support.v7.widget.Toolbar    
               android:layout_width="match_parent"    
               android:layout_height="?attr/actionBarSize"    
               android:id="@+id/toolbar"    
               android:background="?attr/colorPrimary"                                                            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"                                      app:popupTheme="@style/ThemeOverlay.AppCompat.Light"    ></android.support.v7.widget.Toolbar>
  </FrameLayout>    
  <TextView          //第二个子控件，随意        
               android:layout_width="match_parent"        
               android:layout_height="match_parent"        
               android:layout_gravity="start"      //必须指定的属性，left表示菜单在左边，right右,start则是根据系统语言判断        
               android:text="This is menu"        
               android:textSize="30sp"        
               android:background="#FFF"/>
</android.support.v4.widget.DrawerLayout>
```

## ToolBar

1、AndroidManifest.xml定义theme

```xml
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
```

 定义文件res/values/styles.xml

```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
```

属性

```xml
<item name="colorPrimary">@color/colorPrimary</item>                       
<item name="colorPrimaryDark">@color/colorPrimaryDark</item> 
<item name="colorAccent">@color/colorAccent</item>
```

2、activity_main.xml修改

```java
<FrameLayout    
xmlns:android=http://schemas.android.com/apk/res/android    //  
xmlns:tools=http://schemas.android.com/tools             
xmlns:app=http://schemas.android.com/apk/res-auto    //指定命名空间，可以使用app:attribute    
android:layout_width="match_parent"    
  android:layout_height="match_parent"    
    tools:context="a.b.c.materialtest.MainActivity">
    <android.support.v7.widget.Toolbar                                //定义Toolbar控件    
    android:layout_width="match_parent"    
    android:layout_height="?attr/actionBarSize"                   //高度设置为actionBar的高度，    
      android:id="@+id/toolbar"    
        android:background="?attr/colorPrimary"                       //设置背景色    
          android:theme="@style/ThemeOverLay.AppCompat.Dark.ActionBar"  //单独让Toolbar使用深色主题    
            app:popupTheme="@style/Theme.AppCompat.Light"                 //指定Toolbar的菜单项变为淡色    >
              </android.support.v7.widget.Toolbar></FrameLayout>
```

3、修改MainActivity.java

```java
public class MainActivity extends AppCompatActivity {    
  @Override    
  protected void onCreate(Bundle savedInstanceState) {        
    super.onCreate(savedInstanceState);        
    setContentView(R.layout.activity_main);        
    Toolbar toolbar=(Toolbar)findViewById(R.id.toolbar);        
    setSupportActionBar(toolbar);    
  }
}
```

4、修改标题栏的文字，AndroidManifest.xml修改

```xml
android:label="@string/app_name"
```

5、添加按钮
创建文件夹res/menu
创建文件res/menu/toolbar.xml，创建toolbar的按钮样式

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app=http://schemas.android.com/apk/res-auto             //添加以保证向下兼容    
                                           >    
  <item    
        android:id="@+id/backup"    
        android:icon="@mipmap/ic_launcher"                            //按钮图片    
        android:title="Backup"    
        app:showAsAction="always"                                     //alyways表示永远显示在toolbar中    
        />    
  <item        
        android:id="@+id/delete"        
        android:icon="@mipmap/ic_launcher"        
        android:title="Delete"        
        app:showAsAction="ifRoom"                                 //ifRoom表示屏幕空间足够则显示在Toolbar中，不够则显示在菜单中        
        />    
  <item        
        android:id="@+id/settings"        
        android:icon="@mipmap/ic_launcher"        
        android:title="Settings"        
        app:showAsAction="never"                                  //never表示永远显示在菜单中        />
</menu>

```

Toolbar的action按钮只显示图标，菜单中的action按钮只显示文字
修改MainActivity.java

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {    
  getMenuInflater().inflate(R.menu.toolbar,menu);           //加载toolbar.xml菜单文件    
  return true;}@Overridepublic boolean onOptionsItemSelected(MenuItem item) {                             //处理点击事件    
  switch (item.getItemId()){        
    case R.id.backup:            
      Toast.makeText(this,"You clicked Backup",Toast.LENGTH_SHORT).show();            
      break;        
    case R.id.delete:            
      Toast.makeText(this,"You clicked Delete",Toast.LENGTH_SHORT).show();            
      break;        
    case R.id.settings:            
      Toast.makeText(this,"You clicked Settings",Toast.LENGTH_SHORT).show();            
      break;        
    default:            
      break;    
  }    
  return true;
}
```

# DrawerLayout

1、最外层添加控件DrawerLayout

```java
<android.support.v4.widget.DrawerLayout                                                              xmlns:android="http://schemas.android.com/apk/res/android"                                        xmlns:app=http://schemas.android.com/apk/res-auto            //向下兼容的语句                  
   android:id="@+id/drawer_layout"    
   android:layout_width="match_parent"   
   android:layout_height="match_parent">
<FrameLayout                                                     //第一个子控件用于屏幕中显示的内容   
   xmlns:android="http://schemas.android.com/apk/res/android"    
   xmlns:tools="http://schemas.android.com/tools"    
   xmlns:app="http://schemas.android.com/apk/res-auto"    
   android:layout_width="match_parent"    
   android:layout_height="match_parent"    
   tools:context="a.b.c.materialtest.MainActivity">
<android.support.v7.widget.Toolbar    
   android:layout_width="match_parent"    
   android:layout_height="?attr/actionBarSize"   
   android:id="@+id/toolbar"    
   android:background="?attr/colorPrimary"    
   android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"    
   app:popupTheme="@style/ThemeOverlay.AppCompat.Light"    
   >
  </android.support.v7.widget.Toolbar> 
</FrameLayout>    
   <TextView            //第二个子控件随意，但必须要有layout_gravity属性        
      android:layout_width="match_parent"        
      android:layout_height="match_parent"        
      android:layout_gravity="left"     //left被认为是侧边栏,START说明随语言变滑动方向        
      android:text="This is menu"        
      android:textSize="30sp"        
      android:background="#FFF"/>
</android.support.v4.widget.DrawerLayout>
```



2、添加导航按钮

drawable--xxhdpi添加导航按钮图标，修改MainActivity代码

```java
protected void onCreate(Bundle savedInstanceState) {    
  super.onCreate(savedInstanceState);    
  setContentView(R.layout.activity_main);    
  Toolbar toolbar=(Toolbar)findViewById(R.id.toolbar);    
  setSupportActionBar(toolbar);    
  mDrawerLayout=(DrawerLayout)findViewById(R.id.drawer_layout);     //得到DrawerLayout实例    
  ActionBar actionBar=getSupportActionBar();                        //得到ActionBar实例    
  if (actionBar!=null){        
    actionBar.setDisplayHomeAsUpEnabled(true);                    //调用ActionBar的setDisplayHomeAsUpEnabled()方法让导航按钮显示出来        
    actionBar.setHomeAsUpIndicator(R.drawable.ic_menu_white_48dp);//设置图标    
  }
}
public boolean onOptionsItemSelected(MenuItem item) {    
  switch (item.getItemId()){        
    case R.id.backup:            
      Toast.makeText(this,"You clicked Backup",Toast.LENGTH_SHORT).show();            
      break;        
    case R.id.delete:            
      Toast.makeText(this,"You clicked Delete",Toast.LENGTH_SHORT).show();            
      break;        
    case R.id.settings:            
      Toast.makeText(this,"You clicked Settings",Toast.LENGTH_SHORT).show();            
      break;        
    case android.R.id.home:                                   //对HomeAsUp按钮进行处理，它的id永远是android.R.id.home            
      mDrawerLayout.openDrawer(GravityCompat.START);           //openDrawer()方法可以展示菜单，传入参数和XML定义一致为start            
      break;        
    default:            
      break;    
  }    
  return true;
}
```

# NavigationView

1、添加依赖包

```xml
compile 'com.android.support:design:26.0.0-alpha1'
```

圆形头像需要（图片圆形化）

```xml
 compile 'de.hdodenhof:circleimageview:2.1.0'
```

2、创建NavigationView的menu

res/menu文件夹下创建nav_menu.xml文件

```xml
<?xml version="1.0" encoding="utf-8"?><menu xmlns:android="http://schemas.android.com/apk/res/android">    
  <group android:checkableBehavior="single">        
    <item            
          android:id="@+id/nav_call"            
          android:icon="@drawable/ic_local_phone_black_36dp"            
          android:title="Call"            
          />        
    <item            
          android:id="@+id/nav_friends"            
          android:icon="@drawable/ic_people_black_36dp"            
          android:title="Friends"            
          />        
    <item            
          android:id="@+id/nav_location"            
          android:icon="@drawable/ic_place_black_36dp"            
          android:title="Location"            
          />        
    <item            
          android:id="@+id/nav_mail"            
          android:icon="@drawable/ic_mail_outline_black_36dp"            
          android:title="Mail"            
          />        
    <item            
          android:id="@+id/nav_task"            
          android:icon="@drawable/ic_perm_contact_calendar_black_36dp"            
          android:title="Tasks"            
          />    
  </group>
</menu>
```

3、创建headerLayout
res/layout文件夹下创建nav_header.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout 
                xmlns:android="http://schemas.android.com/apk/res/android"    
                android:layout_width="match_parent"    
                android:layout_height="180dp"                //NavigationView适合的高度    
                android:padding="10dp"    
                android:background="?attr/colorPrimary"    
                >
 <de.hdodenhof.circleimageview.CircleImageView    
                android:layout_width="70dp"    
                android:layout_height="70dp"    
                android:id="@+id/icon_image"                                                                       android:src="@drawable/ic_account_circle_white_36dp"    
                android:layout_centerInParent="true" />    
  <TextView        
            android:layout_width="wrap_content"        
            android:layout_height="wrap_content"        
            android:id="@+id/username"        
            android:layout_alignParentBottom="true"        
            android:text="gmail.com"        
            android:textColor="#FFF"        
            android:textSize="14sp"        
            />    
  <TextView        
            android:layout_width="wrap_content"        
            android:layout_height="wrap_content"        
            android:id="@+id/mail"        
            android:layout_above="@+id/username"        
            android:text="Tony"        
            android:textColor="#FFF"        
            android:textSize="14sp"        
            />
</RelativeLayout>
```

3、activity_main.xml添加NavigationView

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto"    
   xmlns:tools="http://schemas.android.com/tools"    
   android:id="@+id/drawer_layout"    
   android:layout_width="match_parent"    
   android:layout_height="match_parent"    
   tools:openDrawer="start">
  <FrameLayout    
               android:id="@+id/frame"    
               android:layout_width="match_parent"    
               android:layout_height="match_parent"    
               >
    <android.support.v7.widget.Toolbar    
             android:id="@+id/toolbar"    
             android:layout_width="match_parent"    
             android:layout_height="?attr/actionBarSize"    
             android:background="?attr/colorPrimary"                                                            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"                                      app:popupTheme="@style/ThemeOverlay.AppCompat.Light"    />
  </FrameLayout><android.support.design.widget.NavigationView        
             android:id="@+id/nav_view"        
             android:layout_width="match_parent"        
             android:layout_height="match_parent"        
             android:layout_gravity="start"        
             app:menu="@menu/nav_menu"        
             app:headerLayout="@layout/nav_header"        /></android.support.v4.widget.DrawerLayout>

```

4、处理菜单项点击事件
修改MainActivity.java

```java
protected void onCreate(Bundle savedInstanceState) {    
  super.onCreate(savedInstanceState);    
  setContentView(R.layout.activity_main);    
  Toolbar toolbar=(Toolbar)findViewById(R.id.toolbar);    
  setSupportActionBar(toolbar);    
  mDrawerLayout=(DrawerLayout)findViewById(R.id.drawer_layout);    
  NavigationView navView=(NavigationView)findViewById(R.id.nav_view);    //获取NavigationView的实例 
  ActionBar actionBar=getSupportActionBar();    
  if (actionBar!=null){        
    actionBar.setDisplayHomeAsUpEnabled(true);        
    actionBar.setHomeAsUpIndicator(R.drawable.ic_menu_white_48dp);    
  }    
  navView.setCheckedItem(R.id.nav_call);  //调用setCheckedItem()方法设置Call菜单项为默认选中    
  navView.setNavigationItemSelectedListener(new NavigationView.OnNavigationItemSelectedListener() {        
    @Override        
    public boolean onNavigationItemSelected( MenuItem item) {            
      mDrawerLayout.closeDrawers();   //菜单项点击事件，无逻辑情况下点击任意菜单项都会关闭DrawerLayout            
      return true;        
    }    
  });
}
```

# FloatingActionButton

1、添加控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto"    
   xmlns:tools="http://schemas.android.com/tools"    
   android:id="@+id/drawer_layout"    
   android:layout_width="match_parent"    
   android:layout_height="match_parent"    
   tools:openDrawer="start">
<FrameLayout    
   android:id="@+id/frame"    
   android:layout_width="match_parent"    
   android:layout_height="match_parent"    
  >
<android.support.v7.widget.Toolbar    
   android:id="@+id/toolbar"    
   android:layout_width="match_parent"    
   android:layout_height="?attr/actionBarSize"    
   android:background="?attr/colorPrimary"                                                            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"                                      app:popupTheme="@style/ThemeOverlay.AppCompat.Light"    
   />    
  <android.support.design.widget.FloatingActionButton        
       android:id="@+id/fab"        
       android:layout_width="wrap_content"        
       android:layout_height="wrap_content"        
       android:layout_gravity="bottom|end"//指定控件放于屏幕右下角，end和NavigationView的start的道理一样    
       android:layout_margin="50dp"                    //表示和屏幕的间距        
       app:elevation="8dp"                             //高度值，值越大阴影感越强        
       app:fabSize="normal"                            //按钮大小                                          app:srcCompat="@drawable/ic_add_white_48dp" />  //按钮上的图片
  </FrameLayout>
  <android.support.design.widget.NavigationView        
       android:id="@+id/nav_view"                   
       android:layout_width="match_parent"        
       android:layout_height="match_parent"        
       android:layout_gravity="start"        
       app:menu="@menu/nav_menu"        
       app:headerLayout="@layout/nav_header"        
                                                />
</android.support.v4.widget.DrawerLayout>
```

2、点击事件

```java
protected void onCreate(Bundle savedInstanceState) {    
  super.onCreate(savedInstanceState);    
  setContentView(R.layout.activity_main);    
  Toolbar toolbar=(Toolbar)findViewById(R.id.toolbar);    
  setSupportActionBar(toolbar);    
  mDrawerLayout=(DrawerLayout)findViewById(R.id.drawer_layout);    
  NavigationView navView=(NavigationView)findViewById(R.id.nav_view);    
  FloatingActionButton fab=(FloatingActionButton)findViewById(R.id.fab);                //获取实例    
  ActionBar actionBar=getSupportActionBar();    
  if (actionBar!=null){        
    actionBar.setDisplayHomeAsUpEnabled(true);        
    actionBar.setHomeAsUpIndicator(R.drawable.ic_menu_white_48dp);    
  }    
  navView.setCheckedItem(R.id.nav_call);    
  navView.setNavigationItemSelectedListener(new NavigationView.OnNavigationItemSelectedListener() {        
    @Override        
    public boolean onNavigationItemSelected( MenuItem item) {            
      mDrawerLayout.closeDrawers();            
      return true;        
    }    
  });    
  fab.setOnClickListener(new View.OnClickListener() {      //和一般的按钮没有什么不同        
    @Override        
    public void onClick(View view) {            
      Toast.makeText(MainActivity.this,"FAB Clicked",Toast.LENGTH_SHORT).show();        
    }    
  });
}
```

# SnackBar

能互动的提示栏

更改点击事件

```java
fab.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Snackbar.make(view,"Data deleted",Snackbar.LENGTH_SHORT)//make方法创建一个Snackbar对象，传入view参数，当前布局任何view都行，参数2是Snackbar中展示的内容，第三个参数为展示的时长
                .setAction("Undo", new View.OnClickListener() { //setAction方法设置动作，按钮文字为Undo，点击Undo显示消息
                    @Override
                    public void onClick(View view) {
                        Toast.makeText(MainActivity.this,"Data restored",Toast.LENGTH_SHORT).show();
                    }
                })
                .show();
    }
});
```

缺陷：会遮住FloatActionButton，需借助CoordinatorLayout解决 

# CoordinatorLayout

加强版FrameLayout，普通情况下和FrameLayout一样，但会监控所有子控件的事件，做出合理响应，例如：弹出Snackbar则FloatActionBar向上偏移

1、更改布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:openDrawer="start">
<android.support.design.widget.CoordinatorLayout   //
    android:id="@+id/frame"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >
<android.support.v7.widget.Toolbar
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    />

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="50dp"
        app:elevation="8dp"
        app:fabSize="normal"
        app:srcCompat="@drawable/ic_add_white_48dp" />
</android.support.design.widget.CoordinatorLayout>    //
<android.support.design.widget.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:menu="@menu/nav_menu"
        app:headerLayout="@layout/nav_header"
        />

</android.support.v4.widget.DrawerLayout>
```

# CardView

1、添加依赖

```xml
compile 'com.android.support:cardview-v7:26.0.0-alpha1'
compile 'com.android.support:recyclerview-v7:26.0.0-alpha1'
compile 'com.github.bumptech.glide:glide:4.0.0-RC1'
```

(glide:强大的图片加载库)
2、布局内添加RecyclerView

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto"                                                xmlns:tools="http://schemas.android.com/tools"    
   android:id="@+id/drawer_layout"    
   android:layout_width="match_parent"    
   android:layout_height="match_parent"    
   tools:openDrawer="start">
  <android.support.design.widget.CoordinatorLayout    
   android:id="@+id/frame"    
   android:layout_width="match_parent"    
   android:layout_height="match_parent"    >
  <android.support.v7.widget.Toolbar    
   android:id="@+id/toolbar"    
   android:layout_width="match_parent"    
   android:layout_height="?attr/actionBarSize"    
   android:background="?attr/colorPrimary"    
   android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"                                      app:popupTheme="@style/ThemeOverlay.AppCompat.Light"    />    <android.support.v7.widget.RecyclerView            //让RecyclerView占满整个空间                            android:layout_width="match_parent"        
         android:layout_height="match_parent"        
         android:id="@+id/recycle_view"         />       //    <android.support.design.widget.FloatingActionButton        
         android:id="@+id/fab"        
         android:layout_width="wrap_content"        
         android:layout_height="wrap_content"        
         android:layout_gravity="bottom|end"        
         android:layout_margin="40dp"        
         app:elevation="8dp"        
         app:fabSize="normal"        
         app:srcCompat="@drawable/ic_add_white_48dp" />
  </android.support.design.widget.CoordinatorLayout>
  <android.support.design.widget.NavigationView        
         android:id="@+id/nav_view"        
         android:layout_width="match_parent"        
         android:layout_height="match_parent"        
         android:layout_gravity="start"        
         app:menu="@menu/nav_menu"        
         app:headerLayout="@layout/nav_header"        />
</android.support.v4.widget.DrawerLayout>
```

3、定义Fruit类

```java
public class Fruit {    
  private String name;    
  private int imageId;    
  public Fruit(String name,int imageId){        
    this.name=name;        
    this.imageId=imageId;    
  }    
  public String getName(){        
    return name;    
  }    
  public int getImageId(){        
    return imageId;    
  }
}
```

3、为RecyclerView指定自定义布局CardView,新建fruit_item.xml

```xml
<?xml version="1.0" encoding="utf-8"?><android.support.v7.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto"    
 android:layout_width="match_parent"                                                                android:layout_height="wrap_content"    
 android:layout_margin="5dp"    
 app:cardCornerRadius="4dp"    >    
  <LinearLayout        
                android:layout_width="match_parent"        
                android:layout_height="wrap_content"        
                android:orientation="vertical"        >        
    <ImageView            
               android:layout_width="match_parent"            
               android:layout_height="100dp"            
               android:id="@+id/fruit_image"            
               android:scaleType="centerCrop"    //指定图片缩放模式，centerCrop让图片保持原有比例充满ImageView,并将超出屏幕的部分裁剪掉            />        
    <TextView            
              android:layout_width="wrap_content"            
              android:layout_height="wrap_content"            
              android:id="@+id/fruit_name"            
              android:layout_gravity="center_horizontal"            
              android:layout_margin="5dp"            
              android:textSize="16sp"            />    
  </LinearLayout>
</android.support.v7.widget.CardView>
```

4、新建适配器,FruitAdapter，泛型指定为FruitAdapter.ViewHolder

```java
public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {    
  private Context mContext;    
  private List<Fruit> mFruitList;    
  @Override    
  public void onBindViewHolder(ViewHolder holder, int position) {      
    Fruit fruit=mFruitList.get(position);                           
    holder.fruitName.setText(fruit.getName());        
    Glide.with(mContext).load(fruit.getImageId()).into(holder.fruitImage);       //和之前的FruitAdapter几乎一样，但使用Glide加载水果图片    
  }                           //Glide.with()方法传入一个Context,Activity,Fragment参数，调用load()方法加载图片，into()方法将图片设置到具体的ImageView    
  @Override    
  public int getItemCount() {        
    return mFruitList.size();    
  }    
  public class ViewHolder extends RecyclerView.ViewHolder{        
    CardView cardView;        
    ImageView fruitImage;        
    TextView fruitName;        
    public ViewHolder(View itemView) {            
      super(itemView);            
      cardView=(CardView) itemView;            
      fruitName=(TextView)itemView.findViewById(R.id.fruit_name);            
      fruitImage=(ImageView)itemView.findViewById(R.id.fruit_image);        
    }    
  }    
  public FruitAdapter(List<Fruit> fruitList){        
    mFruitList=fruitList;    
  }    
  public ViewHolder onCreateViewHolder(ViewGroup parent,int viewType){        
    if(mContext==null){            
      mContext=parent.getContext();        
    }        
    View view= LayoutInflater.from(mContext).inflate(R.layout.fruit_item,parent,false);        return new ViewHolder(view);    
  }
}
```

5、修改MainActivity

```java
public class MainActivity extends AppCompatActivity {    
  private DrawerLayout mDrawerLayout;    
  private Fruit[] fruits={                                //定义数组，存放Fruit实例            
    new Fruit("Apple",R.drawable.apple),            
    new Fruit("Banana",R.drawable.banana),            
    new Fruit("Orange",R.drawable.orange),            
    new Fruit("Watermelon",R.drawable.watermelon),            
    new Fruit("Pear",R.drawable.pear),            
    new Fruit("Grape",R.drawable.grape),            
    new Fruit("Pineapple",R.drawable.pineapple),            
    new Fruit("Strawberry",R.drawable.strawberry),            
    new Fruit("Cherry",R.drawable.cherry),            
    new Fruit("Mango",R.drawable.mango),    
  };    
  private List<Fruit> fruitList=new ArrayList<>();    
  private FruitAdapter adapter;    
  @Override    
  protected void onCreate(Bundle savedInstanceState) {        
    super.onCreate(savedInstanceState);        
    setContentView(R.layout.activity_main);        
    Toolbar toolbar=(Toolbar)findViewById(R.id.toolbar);        
    setSupportActionBar(toolbar);        
    mDrawerLayout=(DrawerLayout)findViewById(R.id.drawer_layout);    
    NavigationView navView=(NavigationView)findViewById(R.id.nav_view);       
    FloatingActionButton fab=(FloatingActionButton)findViewById(R.id.fab);   
    ActionBar actionBar=getSupportActionBar();    
    if (actionBar!=null){      
      actionBar.setDisplayHomeAsUpEnabled(true);            
      actionBar.setHomeAsUpIndicator(R.drawable.ic_menu_white_48dp);       
    }      
    navView.setCheckedItem(R.id.nav_call);        
    navView.setNavigationItemSelectedListener(new NavigationView.OnNavigationItemSelectedListener() {            
      @Override            
      public boolean onNavigationItemSelected( MenuItem item) {      
        mDrawerLayout.closeDrawers();           
        return true;       
      }      
    });    
    fab.setOnClickListener(new View.OnClickListener() {     
      @Override         
      public void onClick(View view) {          
        Snackbar.make(view,"Data deleted",Snackbar.LENGTH_SHORT)                      
          .setAction("Undo", new View.OnClickListener() {        
          @Override                   
          public void onClick(View view) {                                
            Toast.makeText(MainActivity.this,"Data restored",Toast.LENGTH_SHORT).show();                            }                      
        })      
          .show();            
      }        
    });        
    initFruits();        
    RecyclerView recycleView=(RecyclerView)findViewById(R.id.recycle_view);        
    GridLayoutManager layoutManager=new GridLayoutManager(this,2);        
    recycleView.setLayoutManager(layoutManager);   
    adapter=new FruitAdapter(fruitList);  
    recycleView.setAdapter(adapter);   
  }   
  private void initFruits() {                           //initFrit()方法中，清空fruitList数据        fruitList.clear();        
    for (int i=0;i<50;i++){                           //随机挑选50个水果            
      Random random=new Random();                   //随机函数            
      int index=random.nextInt(fruits.length);      //取出随机数，赋值给index            
      fruitList.add(fruits[index]);                 //fruitList随机添加Fruit数组内的实例        
    }   
  }   
  @Override    
  public boolean onCreateOptionsMenu(Menu menu) {        
    getMenuInflater().inflate(R.menu.toolbar,menu);     
    return true; 
  }   
  @Override  
  public boolean onOptionsItemSelected(MenuItem item) {  
    switch (item.getItemId()){       
      case R.id.backup:        
        Toast.makeText(this,"You clicked Backup",Toast.LENGTH_SHORT).show();      
        break;          
      case R.id.delete:         
        Toast.makeText(this,"You clicked Delete",Toast.LENGTH_SHORT).show();   
        break;         
      case R.id.settings:          
        Toast.makeText(this,"You clicked Settings",Toast.LENGTH_SHORT).show();               
        break;        
      case android.R.id.home:        
        mDrawerLayout.openDrawer(GravityCompat.START);        
        break;     
      default:    
    }       
    return true;  
  }
}
```

存在缺陷：下滑RecyclerView会遮挡Toolbar 

![defect](https://cvbnt.github.io/cvbnt.github.io/assets/images/defect.png)

原因：CoordinatorLayout类似FrameLayout，其中的所有控件在不进行明确定位情况下默认摆在布局左上角

# AppBarLayout

AppBarLayout为是垂直方向的LinearLayout

1、解决RecyclerView覆盖toolbar的问题

```xml
<?xml version="1.0" encoding="utf-8"?><android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto"    xmlns:tools="http://schemas.android.com/tools"    
 android:id="@+id/drawer_layout"    
 android:layout_width="match_parent"    
 android:layout_height="match_parent"    
 tools:openDrawer="start">
  <android.support.design.widget.CoordinatorLayout    
 android:id="@+id/frame"    
 android:layout_width="match_parent"    
 android:layout_height="wrap_content">   
  <android.support.design.widget.AppBarLayout   //将Toolbar放置在AppBarLayout里                      android:layout_width="match_parent"        
 android:layout_height="wrap_content">  
  <android.support.v7.widget.Toolbar    
 android:id="@+id/toolbar"    
 android:layout_width="match_parent"    
 android:layout_height="?attr/actionBarSize"   
 android:background="?attr/colorPrimary"    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"        app:layout_scrollFlags="scroll|enterAlways|snap"    />    </android.support.design.widget.AppBarLayout>  //    
    <android.support.v7.widget.RecyclerView        
       android:layout_width="match_parent"       
       android:layout_height="match_parent"        
       android:id="@+id/recycle_view"                                                                    app:layout_behavior="@string/appbar_scrolling_view_behavior" //RecyclerView使用app:layout_behaviour指定布局行为        />    
    <android.support.design.widget.FloatingActionButton        
       android:id="@+id/fab"       
       android:layout_width="wrap_content"       
       android:layout_height="wrap_content"       
       android:layout_gravity="bottom|end"     
       android:layout_margin="40dp"       
       app:elevation="8dp"     
       app:fabSize="normal"       
       app:srcCompat="@drawable/ic_add_white_48dp" />
  </android.support.design.widget.CoordinatorLayout>
  <android.support.design.widget.NavigationView     
       android:id="@+id/nav_view"      
       android:layout_width="match_parent"  
       android:layout_height="match_parent"      
       android:layout_gravity="start"     
       app:menu="@menu/nav_menu"     
       app:headerLayout="@layout/nav_header"        />
</android.support.v4.widget.DrawerLayout>
```

2、向上滚动让Toolbar自动消失

```xml
<?xml version="1.0" encoding="utf-8"?><android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto"    xmlns:tools="http://schemas.android.com/tools"    
 android:id="@+id/drawer_layout"   
 android:layout_width="match_parent"  
 android:layout_height="match_parent"  
 tools:openDrawer="start">
  <android.support.design.widget.CoordinatorLayout   
  android:id="@+id/frame"   
  android:layout_width="match_parent"  
  android:layout_height="wrap_content"> 
    <android.support.design.widget.AppBarLayout     
       android:layout_width="match_parent"    
       android:layout_height="wrap_content">   
    <android.support.v7.widget.Toolbar  
      android:id="@+id/toolbar" 
      android:layout_width="match_parent"  
      android:layout_height="?attr/actionBarSize"  
      android:background="?attr/colorPrimary"                                                           android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"                                       app:popupTheme="@style/ThemeOverlay.AppCompat.Light"                                               app:layout_scrollFlags="scroll|enterAlways|snap"     /> //scroll表示RecyclerView向上滚动，Toolbar自动隐藏，enterAlyways表示向下滚动则Toolbar一直显示，snap表示当Toolbar不完全显示则根据滚动距离自动选择隐藏和显示    
    </android.support.design.widget.AppBarLayout> 
    <android.support.v7.widget.RecyclerView    
      android:layout_width="match_parent"   
      android:layout_height="match_parent"    
      android:id="@+id/recycle_view"                                                                     app:layout_behavior="@string/appbar_scrolling_view_behavior"          />    <android.support.design.widget.FloatingActionButton  
      android:id="@+id/fab"        
      android:layout_width="wrap_content"     
      android:layout_height="wrap_content"        
      android:layout_gravity="bottom|end"    
      android:layout_margin="40dp"        
      app:elevation="8dp"       
      app:fabSize="normal"        
      app:srcCompat="@drawable/ic_add_white_48dp" />
  </android.support.design.widget.CoordinatorLayout>
  <android.support.design.widget.NavigationView  
      android:id="@+id/nav_view"        
      android:layout_width="match_parent"    
      android:layout_height="match_parent"        
      android:layout_gravity="start"     
      app:menu="@menu/nav_menu"        
      app:headerLayout="@layout/nav_header"        />
</android.support.v4.widget.DrawerLayout>
```

# 下拉刷新

1、编辑布局

```xml
<?xml version="1.0" encoding="utf-8"?><android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto"    xmlns:tools="http://schemas.android.com/tools"    
 android:id="@+id/drawer_layout"    
 android:layout_width="match_parent"    
 android:layout_height="match_parent"    
 tools:openDrawer="start">
  <android.support.design.widget.CoordinatorLayout    
   android:id="@+id/frame"    
   android:layout_width="match_parent"    
   android:layout_height="wrap_content">   
    <android.support.design.widget.AppBarLayout     
     android:layout_width="match_parent"       
     android:layout_height="wrap_content">  
    <android.support.v7.widget.Toolbar  
      android:id="@+id/toolbar"  
      android:layout_width="match_parent"   
      android:layout_height="?attr/actionBarSize" 
      android:background="?attr/colorPrimary"                                                           android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"        b                             app:popupTheme="@style/ThemeOverlay.AppCompat.Light"                                               app:layout_scrollFlags="scroll|enterAlways|snap"    />    </android.support.design.widget.AppBarLayout>
    <android.support.v4.widget.SwipeRefreshLayout                   //将RecyclerView包含进去             android:layout_width="match_parent"    
      android:layout_height="match_parent"     
      android:id="@+id/swipe_refresh"                                                                   app:layout_behavior="@string/appbar_scrolling_view_behavior">    <android.support.v7.widget.RecyclerView  
      android:layout_width="match_parent"    
      android:layout_height="match_parent"    
      android:id="@+id/recycle_view"                                                                     app:layout_behavior="@string/appbar_scrolling_view_behavior"        />    </android.support.v4.widget.SwipeRefreshLayout>                 //    <android.support.design.widget.FloatingActionButton     
      android:id="@+id/fab"        
      android:layout_width="wrap_content"    
      android:layout_height="wrap_content"        
      android:layout_gravity="bottom|end"     
      android:layout_margin="40dp"        
      app:elevation="8dp"     
      app:fabSize="normal"        
      app:srcCompat="@drawable/ic_add_white_48dp" />
  </android.support.design.widget.CoordinatorLayout>
  <android.support.design.widget.NavigationView     
      android:id="@+id/nav_view"        
      android:layout_width="match_parent"     
      android:layout_height="match_parent"       
      android:layout_gravity="start"     
      app:menu="@menu/nav_menu"        
      app:headerLayout="@layout/nav_header"        />
</android.support.v4.widget.DrawerLayout>
```

2、MainActivity添加逻辑

```java
public class MainActivity extends AppCompatActivity {    
  private DrawerLayout mDrawerLayout;    
  private SwipeRefreshLayout swipeRefresh;               //    
  private Fruit[] fruits={            
    new Fruit("Apple",R.drawable.apple),            
    new Fruit("Banana",R.drawable.banana),     
    new Fruit("Orange",R.drawable.orange),      
    new Fruit("Watermelon",R.drawable.watermelon),      
    new Fruit("Pear",R.drawable.pear),        
    new Fruit("Grape",R.drawable.grape),        
    new Fruit("Pineapple",R.drawable.pineapple),   
    new Fruit("Strawberry",R.drawable.strawberry),  
    new Fruit("Cherry",R.drawable.cherry),        
    new Fruit("Mango",R.drawable.mango),  
  };   
  private List<Fruit> fruitList=new ArrayList<>();  
  private FruitAdapter adapter;  
  @Override  
  protected void onCreate(Bundle savedInstanceState) {      
    super.onCreate(savedInstanceState);    
    setContentView(R.layout.activity_main);    
    Toolbar toolbar=(Toolbar)findViewById(R.id.toolbar);    
    setSupportActionBar(toolbar);       
    mDrawerLayout=(DrawerLayout)findViewById(R.id.drawer_layout);   
    NavigationView navView=(NavigationView)findViewById(R.id.nav_view);     
    FloatingActionButton fab=(FloatingActionButton)findViewById(R.id.fab);    
    swipeRefresh=(SwipeRefreshLayout)findViewById(R.id.swipe_refresh);    //        
    swipeRefresh.setColorSchemeResources(R.color.colorPrimary);           //设置下拉刷新圆环颜色    
    swipeRefresh.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() { //下拉刷新监听器 
      @Override       
      public void onRefresh() {         //一下拉则调用onRefresh()方法      
        refreshFruits();        
      }    
    });     
    ActionBar actionBar=getSupportActionBar();    
    if (actionBar!=null){         
      actionBar.setDisplayHomeAsUpEnabled(true);      
      actionBar.setHomeAsUpIndicator(R.drawable.ic_menu_white_48dp);     
    }      
    navView.setCheckedItem(R.id.nav_call);    
    navView.setNavigationItemSelectedListener(new NavigationView.OnNavigationItemSelectedListener() {     
      @Override      
      public boolean onNavigationItemSelected( MenuItem item) {               
        mDrawerLayout.closeDrawers();         
        return true;      
      }      
    });      
    fab.setOnClickListener(new View.OnClickListener() {           
      @Override       
      public void onClick(View view) {           
        Snackbar.make(view,"Data deleted",Snackbar.LENGTH_SHORT)                        .setAction("Undo", new View.OnClickListener() {         
          @Override               
          public void onClick(View view) {                                
            Toast.makeText(MainActivity.this,"Data restored",Toast.LENGTH_SHORT).show();                            }            
        })                  
          .show();        
      }   
    });        
    initFruits();    
    RecyclerView recycleView=(RecyclerView)findViewById(R.id.recycle_view);        
    GridLayoutManager layoutManager=new GridLayoutManager(this,2);        
    recycleView.setLayoutManager(layoutManager);    
    adapter=new FruitAdapter(fruitList);   
    recycleView.setAdapter(adapter);  
  }   
  private void refreshFruits() {   
    new Thread(new Runnable() {                 //开启子进程         
      @Override          
      public void run() {            
        try {              
          Thread.sleep(2000);             //线程沉睡2秒钟(不设置则本地刷新太快，刷新立刻结束看不到刷新过程)         
        }catch (InterruptedException e){           
          e.printStackTrace();   
        }            
        runOnUiThread(new Runnable() {      //切回主线程           
          @Override                   
          public void run() {         
            initFruits();                     //调用initFruits方法重新生成数据                       
            adapter.notifyDataSetChanged();   //通知数据发生变化                        
            swipeRefresh.setRefreshing(false);//传入false表示刷新结束，隐藏刷新进度条                 
          }              
        });       
      }      
    }).start();  
  }

```

# 可折叠式标题栏

CollapsingToolbarLayout必须是AppBarLayout的子布局，而AppBarLayout必须是CoordinatorLayout的子布局

1、 新建activity_fruit.xml

```xml
<?xml version="1.0" encoding="utf-8"?><android.support.design.widget.CoordinatorLayout                   //最外层布局是CoordinatorLayout                                                               xmlns:android="http://schemas.android.com/apk/res/android"                                         xmlns:app="http://schemas.android.com/apk/res-auto"   
android:layout_width="match_parent"   
android:layout_height="match_parent">  
  <android.support.design.widget.AppBarLayout                    //第二层布局是AppBarLayout       
    android:layout_width="match_parent"       
    android:layout_height="250dp"                              //高度为250dp视觉效果较好       
    android:id="@+id/appBar">    
    <android.support.design.widget.CollapsingToolbarLayout     //第三层是可折叠式标题栏            android:layout_width="match_parent"      
 android:layout_height="match_parent"     
 android:id="@+id/collapsing_toolbar"            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"          
 app:contentScrim="?attr/colorPrimary"                  //指定标题栏折叠中和折叠后的背景色         
 app:layout_scrollFlags="scroll|exitUntilCollapsed">    //scroll表示可折叠式标题栏会随着水果内容详情的滚动一起滚动，exitUntilCollapsed表示滚动完折叠完后保留在界面上不移出屏幕            
    <ImageView                                             //添加具体内容，一个ImageView和ToolBar                android:layout_width="match_parent"   
               android:layout_height="match_parent"     
               android:id="@+id/fruit_image_view"     
               android:scaleType="centerCrop"    
               app:layout_collapseMode="parallax"/>//折叠过程中的模式，parallax表示折叠过程中有一定偏移            
    <android.support.v7.widget.Toolbar          
       android:id="@+id/toolbar"             
       android:layout_width="match_parent"       
       android:layout_height="?attr/actionBarSize"       
       app:layout_collapseMode="pin"/>                   //pin表示折叠过程中位置始终不变        </android.support.design.widget.CollapsingToolbarLayout>    </android.support.design.widget.AppBarLayout>  
  <android.support.v4.widget.NestedScrollView       //层级和AppBarLayout并列，为详细内容展示   
       android:layout_width="match_parent"       
       android:layout_height="match_parent"          
       app:layout_behavior="@string/appbar_scrolling_view_behavior">     
    <LinearLayout                                             //NestedScrollView只允许一个子布局            android:layout_width="match_parent"      
         android:layout_height="wrap_content"    
         android:orientation="vertical">       
      <android.support.v7.widget.CardView                   //放入一个卡片布局，卡片内有TextView                android:layout_width="match_parent"     
           android:layout_height="wrap_content"                
           android:layout_marginBottom="15dp"        
           android:layout_marginLeft="15dp"             
           android:layout_marginRight="15dp"        
           android:layout_marginTop="35dp"             
           app:cardCornerRadius="4dp">           
        <TextView              
          android:layout_width="wrap_content"                    
          android:layout_height="wrap_content"        
          android:id="@+id/fruit_content_text"       
          android:layout_margin="10dp"/>    
      </android.support.v7.widget.CardView>   
    </LinearLayout>
  </android.support.v4.widget.NestedScrollView>    <android.support.design.widget.FloatingActionButton          //悬浮按钮，和AppBarLayout平级，        android:layout_width="wrap_content"      
 android:layout_height="wrap_content"  
 android:layout_margin="16dp"      
 android:src="@drawable/ic_comment"  
 app:layout_anchor="@id/appBar"                           //anchor属性为锚点，设置为AppBarLayout意思为会出现在标题栏区域内      
 app:layout_anchorGravity="bottom|end"/>                  //定位在右下角    </android.support.design.widget.CoordinatorLayout>
```

2、新建FruitActivity.java

```java
public class FruitActivity extends AppCompatActivity {
    public static final String FRUIT_NAME="fruit_name";           //新建字符串水果名字和水果图片id
    public static final String FRUIT_IMAGE_ID="fruit_image_id";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_fruit);
        Intent intent=getIntent();
        String fruitName=intent.getStringExtra(FRUIT_NAME);       //字符串获得数据
        int fruitImageId=intent.getIntExtra(FRUIT_IMAGE_ID,0);   
        Toolbar toolbar=(Toolbar)findViewById(R.id.toolbar);
        CollapsingToolbarLayout collapsingToolbar=(CollapsingToolbarLayout)findViewById(R.id.collapsing_toolbar);
        ImageView fruitImageView=(ImageView)findViewById(R.id.fruit_image_view);
        TextView fruitContentText=(TextView)findViewById(R.id.fruit_content_text);
        setSupportActionBar(toolbar);
        ActionBar actionBar=getSupportActionBar();                //实现ToolBar,作为ActionBar
        if (actionBar!=null){
            actionBar.setDisplayHomeAsUpEnabled(true);            //启用HomeAsUp按钮，默认图标为返回箭头
        }
        collapsingToolbar.setTitle(fruitName);                    //标题设置为水果名
        Glide.with(this).load(fruitImageId).into(fruitImageView); //Glide传入水果图片，设置到标题栏上的ImageView上
        String fruitContent=generateFruitContent(fruitName);      //创建generateFruitContent方法将水果名循环500次
        fruitContentText.setText(fruitContent);                   //将循环后的字符添加到详情内的TextView内
    }

    private String generateFruitContent(String fruitName) {
        StringBuilder fruitContent=new StringBuilder();
        for (int i=0;i<500;i++){
            fruitContent.append(fruitName);
        }
        return fruitContent.toString();
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()){
            case android.R.id.home:                         //处理HomeAsUp的按钮事件
                finish();                                    //关闭当前活动，返回上个活动
                return true;
        }
        return super.onOptionsItemSelected(item);
    }
}
```

3、处理RecycleView点击事件
修改FruitAdapter.java

```java
public ViewHolder onCreateViewHolder(ViewGroup parent,int viewType){    
  if(mContext==null){        
    mContext=parent.getContext();    
  }    
  View view= LayoutInflater.from(mContext).inflate(R.layout.fruit_item,parent,false);    
  final ViewHolder holder=new ViewHolder(view); 
  holder.cardView.setOnClickListener(new View.OnClickListener() {           //为CardView添加监听器 
    @Override  
    public void onClick(View v) {          
      int position=holder.getAdapterPosition();                         //点击CardView           
      Fruit fruit=mFruitList.get(position);       
      Intent intent=new Intent(mContext,FruitActivity.class);    
      intent.putExtra(FruitActivity.FRUIT_NAME,fruit.getName());        //获取当前水果名和图片资源id
      intent.putExtra(FruitActivity.FRUIT_IMAGE_ID,fruit.getImageId());            mContext.startActivity(intent);      
    }   
  });   
  return holder;
}
```

# 背景图和状态栏融合

1、修改activity_fruit.xml,需要将ImageView融合到状态栏里，则必须将ImageView及它的父布局的android:fitsSystemWindows属性设置为true

```xml
<?xml version="1.0" encoding="utf-8"?><android.support.design.widget.CoordinatorLayout    xmlns:android="http://schemas.android.com/apk/res/android"    xmlns:app="http://schemas.android.com/apk/res-auto" 
android:layout_width="match_parent"   
android:layout_height="match_parent"  
android:fitsSystemWindows="true">                                    //    <android.support.design.widget.AppBarLayout    
android:layout_width="match_parent"     
android:layout_height="250dp"    
android:id="@+id/appBar"    
android:fitsSystemWindows="true">                                //        <android.support.design.widget.CollapsingToolbarLayout            android:layout_width="match_parent"     
android:layout_height="match_parent"      
android:id="@+id/collapsing_toolbar"           
android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" 
app:contentScrim="?attr/colorPrimary"           
app:layout_scrollFlags="scroll|exitUntilCollapsed"                                                 android:fitsSystemWindows="true">                            //      
  <ImageView            
             android:layout_width="match_parent"   
             android:layout_height="match_parent"       
             android:id="@+id/fruit_image_view"     
             android:scaleType="centerCrop"           
             app:layout_collapseMode="parallax"       
             android:fitsSystemWindows="true"/>                       //         
  <android.support.v7.widget.Toolbar        
                                     android:id="@+id/toolbar"    
                                     android:layout_width="match_parent"     
                                     android:layout_height="?attr/actionBarSize"      
                                     app:layout_collapseMode="pin"/>        </android.support.design.widget.CollapsingToolbarLayout>    </android.support.design.widget.AppBarLayout> 
  <android.support.v4.widget.NestedScrollView     
                  android:layout_width="match_parent"       
                  android:layout_height="match_parent"       
                  app:layout_behavior="@string/appbar_scrolling_view_behavior">   
    <LinearLayout       
                  android:layout_width="match_parent"            
                  android:layout_height="wrap_content" 
                  android:orientation="vertical">     
      <android.support.v7.widget.CardView      
                                          android:layout_width="match_parent"                
                                          android:layout_height="wrap_content"     
                                          android:layout_marginBottom="15dp"                
                                          android:layout_marginLeft="15dp"     
                                          android:layout_marginRight="15dp"               
                                          android:layout_marginTop="35dp"        
                                          app:cardCornerRadius="4dp">           
        <TextView               
                  android:layout_width="wrap_content"         
                  android:layout_height="wrap_content"      
                  android:id="@+id/fruit_content_text"     
                  android:layout_margin="10dp"/>       
      </android.support.v7.widget.CardView>      
    </LinearLayout>   
  </android.support.v4.widget.NestedScrollView>    <android.support.design.widget.FloatingActionButton    
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"     
    android:layout_margin="16dp"    
    android:src="@drawable/ic_comment"    
    app:layout_anchor="@id/appBar" 
    app:layout_anchorGravity="bottom|end"/>   
</android.support.design.widget.CoordinatorLayout>
```

2、将程序的主题中的状态栏指定为透明色，属性android:statusBarColor，但该属性只在API21以后才有，则res目录新建values-v21文件夹，values-v21新建styles.xml，编辑

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources> 
  <style name="FruitActivityTheme" parent="AppTheme">  //定义主题名字FruitActivityTheme,继承主题AppTheme，获得所有特性     
    <item name="android:statusBarColor">@android:color/transparent</item>    //设置为透明色      </style>
</resources>
```

3、针对API21之前的系统，编辑res/values/styles.xml

```xml
<resources>    <!-- Base application theme. -->   
  <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">        <!-- Customize your theme here. -->      
    <item name="colorPrimary">@color/colorPrimary</item>      
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>     
    <item name="colorAccent">@color/colorAccent</item> 
  </style>
  <style name="FruitActivityTheme" parent="AppTheme"> //同样定义一个FruitActivityTheme主题，什么都不写则状态栏无颜色
  </style>
</resources>
```

4、让FruitActivity使用该主题，编辑AndroidManifest.xml，添加

```xml
<activity android:name=".FruitActivity"    
android:theme="@style/FruitActivityTheme"></activity>
```

Markdown
*Awesome*