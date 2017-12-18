
---  

layout: post  

title: ContentProvider

date: 2017-12-02 09:00:00 +0800 

categories: AS ContentProvider

---  

## 基本操作
1、ContentResolver提供共享的数据，获取实例    
```JAVA
getContentResolver()   
```
CRUD操作   
```JAVA
insert()添加数据  

update()更新数据  

delete()删除数据  

query()查询数据  
```
2、ContentResolver接收URI  

URI由两部分组成  

authurity和path  

&ensp;&ensp;包名&ensp;&ensp;表名  
content://com.example.app.provider/table1
3、将URI字符串解析为Uri对象
```JAVA
URI uri=Uri.parse("content://com.example.app.provider/table1"）
```
例如
```JAVA
Cursor corsor=getContentResolver().query(
uri,             //指定查询某个应用程序下的一张表
projection,  //制定查询的列名
selection,    //where约束条件
selectionArgs, //为where中的占位符提供值，一般为?
sortOrder);    //指定查询结果排列方式
```
4、查询Cursor对象中的值   
通过游标的位置遍历Cursor所有行，读取每一行的数据   
```JAVA
if(cursor !=null){
while(cursor.moveToNext()){
String column1=cursor.getString(cursor.getColumnIndex("column1"));
int colum2=cursor.getInt(cursor.getColumnIndex("column2"));
}
cursor.close();
}
```
5、增删改  
添加数据  
```JAVA
ContentValues values=new ContentValues();
values.put("column1","text");
values.put("column2",1);     //向两个列名分别为column1和column2插入数据
getContentResolver().insert(uri,values);  //提交
```
更新数据（清空column1的数据）  
```JAVA 
ContentValues values=new ContentValues();
values.put("column1","");
getContentResolver().update(uri,values,column1=? and column2 =?",new String[] {"text","1"});
```
删除数据
```JAVA
getContentResolver().delete(uri,"column2=?".new String[]{"1"});
```
## 例子：读取联系人
1、联系人添加两个号码，布局添加ListView，id为contact_view  

2、MainActivity代码  
```JAVA
public class MainActivity extends AppCompatActivity {    
	ArrayAdapter<String> adapter;                     //定义适配器    
	List<String> contactsList=new ArrayList<>();      //创建联系人List    
	@Override    
	protected void onCreate(Bundle savedInstanceState) {        
		super.onCreate(savedInstanceState);        
		setContentView(R.layout.activity_main);        
		ListView contactsView=(ListView)findViewById(R.id.contacts_view);    //创建ListView对象        
		adapter=new ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,contactsList);  //定义适配器，传入上下文，子项布局id,数据        
		contactsView.setAdapter(adapter);                       //向ListView对象设置适配器        
		if(ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS)!= PackageManager.PERMISSION_GRANTED){            
			ActivityCompat.requestPermissions(this,new String[]{Manifest.permission.READ_CONTACTS},1);        
		}else {            
			readContacts();        
		}    
	}    
	private void readContacts() {      //创建读取联系人方法        
		Cursor cursor=null;        
		try{            
			cursor=getContentResolver().query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI,null,null,null,null);  //查询联系人,联系人常量为ContactsContract.CommonDataKinds.Phone.CONTENT_URI            
			if (cursor!=null){                
				while(cursor.moveToNext()){      //遍历Cursor对象                    
					String displayName=cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME)); //联系人名，常量为ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME                                                          
					String number=cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));//号码，常量ContactsContract.CommonDataKinds.Phone.NUMBER                    
					contactsList.add(displayName+"\n"+number);                 //往list内添加名字换行和号码                     
					    }                
				adapter.notifyDataSetChanged();     //通知刷新ListView            
			}        
		}catch(Exception e){            
		    e.printStackTrace();        
		}finally {            
			if (cursor!=null){                
				cursor.close();              //最后必须关掉cursor            
				}        
		    }    
		}    
	@Override    
	public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {        
		switch (requestCode){            
			case 1:                
					if (grantResults.length>0&&grantResults[0]==PackageManager.PERMISSION_GRANTED){                    
						readContacts();                
					}else {                    
						Toast.makeText(this,"You denied the permission",Toast.LENGTH_SHORT).show();                
				    }                
					break;            
				default:        
			}    
		}
    }
```
3、AndroidManifest.xml添加权限
```XML
<uses-permission android:name="android.permission.READ_CONTACTS"/>
```
## 自定义内容提供器
1、创建新类继承ContentProvider  

2、重写抽象方法  
```JAVA
public class MyProvider extends ContentProvider {    
	@Override    
	public boolean onCreate() {        //初始化内容提供器，返回true表示初始化成功，当存在ContentProvider尝试访问程序的数据时，内容提供器才会初始化        
		return false;    
	}    
	@Nullable    
	@Override    
	public Cursor query(@NonNull Uri uri, @Nullable String[] projection, @Nullable String selection, @Nullable String[] selectionArgs, @Nullable String sortOrder) {        
		//uri参数决定查询哪张表，projection决定查询哪一列，selection和selectionArgs约束查询哪些行，sortOrder用于对结果进行排序，查询结果保存在Cursor中返回        
		return null;    
	}    
	@Nullable    
	@Override    
	public String getType(@NonNull Uri uri) {        //根据传入内容URI返回相应MIME类型        
		return null;    
    }    
	@Nullable    
	@Override    
	public Uri insert(@NonNull Uri uri, @Nullable ContentValues values) {        //添加数据，uri参数确定插入的表，待添加数据保存在values参数中，添加完成后，返回表示这条记录的URI        
		return null;    
	}    
	@Override    
	public int delete(@NonNull Uri uri, @Nullable String selection, @Nullable String[] selectionArgs) {        //同insert()，但返回删除的行数        
		return 0;    
	}    
	@Override    
	public int update(@NonNull Uri uri, @Nullable ContentValues values, @Nullable String selection, @Nullable String[] selectionArgs) {        //同insert()，但返回受影响的行数        
		return 0;    
	}
}
```
## URI
标准URI  

content://com.example.app.provider/table1  

访问com.example.app中table1这个表的数据  

具体URI  

content://com.example.app.provider/table1/1  

访问com.example.app中table1这个表中id为1的数据  

通配符  

content://com.example.app.provider/*   

访问该app种任何表的内容   

content://com.example.app.provider/table1/#  

访问table1表中任意一行数据内容  
## UriMatcher:实现匹配内容URI
```JAVA
public class MyProvider extends ContentProvider {    
	public static final int TABLE1_DIR=0;   //自定义四个参数，四个代码    
	public static final int TABLE1_ITEM=1;    public static final int TABLE2_DIR=2;    
	public static final int TABLE2_ITEM=3;    
	private static UriMatcher uriMatcher;        
	static {        
		uriMatcher=new UriMatcher(UriMatcher.NO_MATCH);                  //创建UriMatcher对象        
		uriMatcher.addURI("a.b.c.ContactsTest","table1",TABLE1_DIR);     //addURI()方法接收三个参数，包名，表名，和自定义的参数        
		uriMatcher.addURI("a.b.c.ContactsTest","table1/#",TABLE1_ITEM);        
		uriMatcher.addURI("a.b.c.ContactsTest","table2",TABLE2_DIR);        
		uriMatcher.addURI("a.b.c.ContactsTest","table2/#",TABLE2_ITEM);    
	}    
	@Override    
	public boolean onCreate() {        //初始化内容提供器，返回true表示初始化成功，当存在ContentProvider尝试访问程序的数据时，内容提供器才会初始化        
		return false;    
	}    
	@Nullable    
	@Override    
	public Cursor query(@NonNull Uri uri, @Nullable String[] projection, @Nullable String selection, @Nullable String[] selectionArgs, @Nullable String sortOrder) {        
		//uri参数决定查询哪张表，projection决定查询哪一列，selection和selectionArgs约束查询哪些行，sortOrder用于对结果进行排序，查询结果保存在Cursor中返回        
		switch (uriMatcher.match(uri)){  //match方法接收Uri对象            
			case TABLE1_DIR:           //查询table1中所有数据                
				break;            
			case TABLE1_ITEM:          //查询table1中单条数据                
				break;            
			case TABLE2_DIR:           //查询table2中所有数据                
				break;            
			case TABLE2_ITEM:          //查询table2中单条数据                
				break;            
			default:                
				break;        
		}        
		return null;    
	}    
}
```
## 内容Uri对应MINE类型  

getType()方法用于获取Uri对象对应的MIME类型  

MIME字符串  

1、vnd.开头  

2、URI以表结尾的，后接android.cursor.dir/，以id结尾的，后接android.cursor.item/  

3、最后接上vnd.<authority>.<path>    

例如：  

URI:content://com.example.app.provider/table1  
  
MIME:vnd.android.cursor.dir/vnd.com.example.app.provider.table1  
  
URI:content://com.example.app.provider/table1/1  
  
MIME:vnd.android.cursor.item/vnd.com.example.app.provider.table1  
```JAVA
public String getType( Uri uri) {
    //根据传入内容URI返回相应MIME类型
    switch (uriMatcher.match(uri)){
        case TABLE1_DIR:
            return "vnd.android.cursor.dir/vnd.a.b.c.ContastsTest.table1";
        case TABLE1_ITEM:
            return "vnd.android.cursor.item/vnd.a.b.c.ContastsTest.table1";
        case TABLE2_DIR:
            return "vnd.android.cursor.dir/vnd.a.b.c.ContastsTest.table2";
        case TABLE2_ITEM:
            return "vnd.android.cursor.item/vnd.a.b.c.ContastsTest.table2";
        default:
            break;
    }
    return null;
}
```
## 应用添加库程序共享，例子
1、原先DatabaseTest中添加自定义ContentProvider类  
```JAVA
public class DatabaseProvider extends ContentProvider {    
	public static final int BOOK_DIR=0;    
	public static final int BOOK_ITEM=1;    
	public static final int CATEGORY_DIR=2;    
	public static final int CATEGORY_ITEM=3;                             //定义4常量让其他APP访问DatabaseTest的数据    
	public static final String AUTHORITY="a.b.c.DatabaseTest.provider";//设置权限名    
	private static UriMatcher uriMatcher;                            //创建UriMatch    
	private MyDatabaseHelper dbHelper;                               //创建数据库对象dbHelpher    
	static {        
		uriMatcher=new UriMatcher(UriMatcher.NO_MATCH);              //设置uriMatcher        
		uriMatcher.addURI(AUTHORITY,"book",BOOK_DIR);                //添加访问book表所有数据的uri        
		uriMatcher.addURI(AUTHORITY,"book/#",BOOK_ITEM);             //添加访问book表单条数据的uri        
		uriMatcher.addURI(AUTHORITY,"category",CATEGORY_DIR);        //添加访问category表所有数据的uri        
		uriMatcher.addURI(AUTHORITY,"category/#",CATEGORY_ITEM);     //添加访问category表单条数据的uri    
	}    
	public DatabaseProvider() {    
	}
	@Override    
	public boolean onCreate() {            
		dbHelper=new MyDatabaseHelper(getContext(),"BookStore.db",null,2);   //dbHelper，填入参数，数据库名，版本号        
		return true;                                                         //返回true说明内容提供器创建成功                      
	}    
	@Override    
	public int delete(Uri uri, String selection, String[] selectionArgs) {              
		SQLiteDatabase db=dbHelper.getWritableDatabase();        
		int deleteRows=0;        
		switch (uriMatcher.match(uri)){                                    //根据传入的uri参数删除            
			case BOOK_DIR:                
				deleteRows=db.delete("Book", selection,selectionArgs);  //删除book表所有的行                
				break;            
			case BOOK_ITEM:                
				String bookId=uri.getPathSegments().get(1);//getPathSegments()方法会将URI权限后的部分以"/"进行分割，将分割后的结果放入字符串列表，第0个位置是路径，第1个位置是id                deleteRows=db.delete("Book","id=?",new String[]{bookId}); //删除book对应的行                
				break;            
			case CATEGORY_DIR:                
				deleteRows=db.delete("Category",selection,selectionArgs);//删除Category表所有的行                
				break;            
			case CATEGORY_ITEM:                
				String categoryId=uri.getPathSegments().get(1);                
				deleteRows=db.delete("Category","id=?",new String[]{categoryId}); //删除Category对应的行                
				break;            
			default:                
				break;        
		}        
		return deleteRows;   //返回删除的行数    
	}    
	@Override    
	public String getType(Uri uri) {        
		switch(uriMatcher.match(uri)){            
			case BOOK_DIR:                
				return "vnd.android.cursor.dir/vnd.a.b.c.DatabaseTest.provider.book";            
			case BOOK_ITEM:                
				return "vnd.android.cursor.item/vnd.a.b.c.DatabaseTest.provider.book";            
			case CATEGORY_DIR:                
				return "vnd.android.cursor.dir/vnd.a.b.c.DatabaseTest.provider.category";            
			case CATEGORY_ITEM:                
				return "vnd.android.cursor.item/vnd.a.b.c.DatabaseTest.provider.category";        
		}        
		return null;    
	}    
	@Override    
	public Uri insert(Uri uri, ContentValues values) {             
		SQLiteDatabase db=dbHelper.getWritableDatabase();        
		Uri uriReturn=null;        
		switch (uriMatcher.match(uri)){            
			case BOOK_DIR:            
			case BOOK_ITEM:                
				long newBookId=db.insert("Book",null,values);                
				uriReturn=Uri.parse("content://"+AUTHORITY+"/book"+newBookId);                
				break;            
			case CATEGORY_DIR:            
			case CATEGORY_ITEM:                
				long newCategoryId=db.insert("Category",null,values);                
				uriReturn=Uri.parse("content://"+AUTHORITY+"/category"+newCategoryId);                
				break;            
			default:                
				break;       
		}        
		return uriReturn;    
	}    
	@Override    
	public Cursor query(Uri uri, String[] projection, String selection,String[] selectionArgs, String sortOrder) {               
		SQLiteDatabase db=dbHelper.getReadableDatabase();        
		Cursor cursor=null;        
		switch (uriMatcher.match(uri)){            
			case BOOK_DIR:                
					cursor=db.query("Book",projection,selection,selectionArgs,null,null,sortOrder);                
					break;            
			case BOOK_ITEM:                
					String bookId=uri.getPathSegments().get(1);                
					cursor=db.query("Book",projection,selection,selectionArgs,null,null,sortOrder);                
					break;            
			case CATEGORY_DIR:                
					cursor=db.query("Category",projection,selection,selectionArgs,null,null,sortOrder);                
					break;            
			case CATEGORY_ITEM:                
					String categoryId=uri.getPathSegments().get(1);                
					cursor=db.query("Category",projection,"id=?",new String[]{categoryId},null,null,sortOrder);                
					break;            
			default:                
			       break;        
		}        
		return cursor;    
	}    
	@Override    
	public int update(Uri uri, ContentValues values, String selection,                      
							String[] selectionArgs) {             
			SQLiteDatabase db=dbHelper.getWritableDatabase();        
			int updatedRows=0;        
			switch (uriMatcher.match(uri)){            
			case BOOK_DIR:                
				updatedRows=db.update("Book",values,selection,selectionArgs);                
				break;            
			case BOOK_ITEM:                
				String bookId=uri.getPathSegments().get(1);                
				updatedRows=db.update("Book",values,"id=?",new String[]{bookId});                
				break;            
			case CATEGORY_DIR:                
				updatedRows=db.update("Category",values,selection,selectionArgs);                
				break;            
			case CATEGORY_ITEM:                
				String categoryId=uri.getPathSegments().get(1);                
				updatedRows=db.update("Category",values,"id=?",new String[]{categoryId});                
				break;            
			default:                
					break;        
		}        
		return updatedRows;    
	}
}
```
2、 新建项目ProviderTest  
布局建立四个按钮  
ADD TO BOOK   
QUERY FROM BOOK   
UPDATE BOOK  
DELETE BOOK  
代码：
```JAVA
public class MainActivity extends AppCompatActivity {    
	private String newId;    
	@Override    
	protected void onCreate(Bundle savedInstanceState) {        
		super.onCreate(savedInstanceState);        
		setContentView(R.layout.activity_main);        
		Button addData=(Button)findViewById(R.id.add_data);       //四种操作，向DatabaseTest的表添加数据        
		Button queryData=(Button)findViewById(R.id.query_data);   //查询数据，log.d()        
		Button updateData=(Button)findViewById(R.id.update_data); //更新数据        
		Button deleteData=(Button)findViewById(R.id.delete_data); //删除数据        
		addData.setOnClickListener(new View.OnClickListener() {            
			@Override            
			public void onClick(View view) {               
				Uri uri=Uri.parse("content://a.b.c.databasetest.provider/book");  //解析uri对象                
				ContentValues values=new ContentValues();                         //将添加的数据添加到ContentValues对象中，                
				values.put("name","A Clash Of Kings");                
				values.put("author","George Martin");                
				values.put("pages",1040);                
				values.put("price",22.85);                
				Uri newUri=getContentResolver().insert(uri,values); //调用insert方法添加数据，并且insert方法会返回一个uri对象，对象包含了新增数据的id                
				newId=newUri.getPathSegments().get(1);              //取出新增数据的id，赋值给newId                         
			}        
		});        
		queryData.setOnClickListener(new View.OnClickListener() {            
			@Override            
			public void onClick(View view) {                
				Uri uri=Uri.parse("content://a.b.c.databasetest.provider/book");                
				Cursor cursor=getContentResolver().query(uri,null,null,null,null);                
				if (cursor!=null){                    
					while (cursor.moveToNext()){                        
						String name=cursor.getString(cursor.getColumnIndex("name"));                        
						String author=cursor.getString(cursor.getColumnIndex("author"));                        
						int pages=cursor.getInt(cursor.getColumnIndex("pages"));                        
						double price=cursor.getDouble(cursor.getColumnIndex("price"));                        
						Log.d("MainActivity","book name is "+name);                        
						Log.d("MainActivity","book author is "+author);                        
						Log.d("MainActivity","book pages is "+pages);                        
						Log.d("MainActivity","book price is "+price);                    
					}                    
					cursor.close();                
				}            
			}        
		});        
		updateData.setOnClickListener(new View.OnClickListener() {            
			@Override            
			public void onClick(View view) {                
				Uri uri=Uri.parse("content://a.b.c.databasetest.provider/book/"+newId);//只希望更新刚刚添加的数据，不影响其他行                
				ContentValues values=new ContentValues();                
				values.put("name","A Storm Of Swords");                
				values.put("pages",1216);                
				values.put("price",24.05);                
				getContentResolver().update(uri,values,null,null);            
			}        
		});        
		deleteData.setOnClickListener(new View.OnClickListener() {            
			@Override            
			public void onClick(View view) {                
				Uri uri=Uri.parse("content://a.b.c.databasetest.provider/book/"+newId);                
				getContentResolver().delete(uri,null,null);            
			}        
		});    
	}
}
```







**Markdown**
*Awesome*