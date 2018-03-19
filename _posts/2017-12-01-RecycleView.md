
---  
layout: post  
title: RecycleView
date: 2017-12-01 22:00:00 +0800 
categories: AS RecycleView
--- 
## 1、添加依赖
```XML
implementation 'com.android.support:recyclerview-v7:27.0.2'
```
## 2、修改activity_main.xml代码。添加RecyclerView
```XML
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 
   xmlns:android="http://schemas.android.com/apk/res/android"    
   xmlns:app="http://schemas.android.com/apk/res-auto"    
   xmlns:tools="http://schemas.android.com/tools"    
   android:layout_width="match_parent"    
   android:layout_height="match_parent"    
   tools:context="a.b.c.recycleviewtest.MainActivity">    
   <android.support.v7.widget.RecyclerView        
      android:layout_width="395dp"        
      android:layout_height="587dp"        
      android:id="@+id/recycler_view"        
      tools:layout_editor_absoluteY="8dp"        
      tools:layout_editor_absoluteX="8dp">    
   </android.support.v7.widget.RecyclerView>
</android.support.constraint.ConstraintLayout>
```
## 3、新建水果类
```XML
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
## 4、新建RecyclerView内要显示的布局fruit_item.xml
```XML
<?xml version="1.0" encoding="utf-8"?>
    <LinearLayout
         xmlns:android="http://schemas.android.com/apk/res/android"        
         android:layout_width="match_parent"
         android:layout_height="wrap_content"
         >        
         <ImageView
                android:layout_width="wrap_content"           
                android:layout_height="wrap_content"            
                android:id="@+id/fruit_image"            
                />        
          <TextView           
                 android:layout_width="wrap_content"            android:layout_height="wrap_content"            
                 android:id="@+id/fruit_name"            android:layout_gravity="center_horizontal"            android:layout_marginLeft="10dp"            
               />    
     </LinearLayout>
```
## 5、新建适配器FruitAdapter类
```JAVA
public class FruitAdapter extends RecyclerView.Adapter <FruitAdapter.ViewHolder>{    
private List<Fruit> mFruitList;    
	public class ViewHolder extends RecyclerView.ViewHolder{       //定义内部类ViewHolder继承RecyclerView.ViewHolder
	       ImageView fruitImage;        
	       TextView fruitName;       
	       public ViewHolder(View itemView) {       //并在ViewHolder的构造函数中传入view,通常是RecyclerView的最外层布局            
	         super(itemView);            
	         fruitName=(TextView)itemView.findViewById(R.id.fruit_name);           
	         fruitImage=(ImageView)itemView.findViewById(R.id.fruit_image);        
	     }    
	 }    
	 public FruitAdapter(List<Fruit> fruitList){   //构造函数用于把展示的数据源传进来赋值给mFruitList,后续操作在该数据源上进行操作
	       mFruitList=fruitList;    
	 }    
	 public ViewHolder onCreateViewHolder(ViewGroup parent,int viewType){  //onCreateViewHolder方法用于创建ViewHolder实例        
	       View view= LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item,parent,false); //将fruit_item布局加载进来        
	       ViewHolder holder=new ViewHolder(view);    //创建ViewHolder实例        
	       return holder;    
	 }    
	 @Override    
	 public void onBindViewHolder(ViewHolder holder, int position) {   //该方法用于对RecyclerView子项数据进行赋值，会在子项滚动到屏幕时执行       
	       Fruit fruit=mFruitList.get(position);        //position参数得到当前Fruit实例        
	       holder.fruitName.setText(fruit.getName());     //将数据设置到ImageView和TextView中        
	       holder.fruitImage.setImageResource(fruit.getImageId());    
	 }    
	 @Override    
	 public int getItemCount() {      //该方法告诉你RecyclerView一共有多少子项        
	       return mFruitList.size();    
     }
}
```
## 6、修改MainActivity
```JAVA
public class MainActivity extends AppCompatActivity {    
	private List<Fruit> fruitList=new ArrayList<>();    
	@Override    
	protected void onCreate(Bundle savedInstanceState) {        
		super.onCreate(savedInstanceState);        
		setContentView(R.layout.activity_main);        
		initFruits();                                                              //初始化水果数据，赋值和图片        
		RecyclerView recyclerView=(RecyclerView)findViewById(R.id.recycler_view);  //创建RecyclerView实例        
		LinearLayoutManager layoutManager=new LinearLayoutManager(this);           //LayoutManager用于指定RecyclerView的布局方式，使用LinearLayoutManager是线性布局的意思，实现了ListView的效果        
		recyclerView.setLayoutManager(layoutManager);         
		FruitAdapter adapter=new FruitAdapter(fruitList);        
		recyclerView.setAdapter(adapter);    
		}    
		private void initFruits() {        
			for (int i=0;i<2;i++){                                                         
				Fruit apple=new Fruit("Apple",R.drawable.apple);            
				fruitList.add(apple);            
				Fruit banana=new Fruit("Banana",R.drawable.banana);            
				fruitList.add(banana);            
				Fruit orange=new Fruit("Orange",R.drawable.orange);            
				fruitList.add(orange);            
				Fruit watermelon=new Fruit("Watermelon",R.drawable.watermelon);            
				fruitList.add(watermelon);            
				Fruit pear=new Fruit("Pear",R.drawable.pear);            
				fruitList.add(pear);            
				Fruit grape=new Fruit("Grape",R.drawable.grape);            
				fruitList.add(grape);            
				Fruit pineapple=new Fruit("Pineapple",R.drawable.pineapple);            
				fruitList.add(pineapple);            
				Fruit strawberry=new Fruit("Strawberry",R.drawable.strawberry);            
				fruitList.add(strawberry);            
				Fruit cherry=new Fruit("Cherry",R.drawable.cherry);            
				fruitList.add(cherry);            
				Fruit mango=new Fruit("Mango",R.drawable.mango);            
				fruitList.add(mango);        
				}    
				}
			}
```
ViewHolder类（容纳View视图，由RecyclerView创建，ViewHolder引用itemView）：   
典型ViewHolder子类   
```JAVA
public class ListRow extends RecyclerView.ViewHolder{    
	public ImageView mThumbnail;    
	public ListRow(View view){           //构造方法传入view,调用父类ViewHolder传入view       
	 super(view);        
	 mThumbnail=(ImageView)findViewById(R.id.thumbnail);    
	 }
	}
```
使用用例   
```JAVA
ListRow row=new ListRow(Inflater.inflate(R.layout.list_row,parent,false));  //传入
View view=row.itemView;
ImageView thumbnailView=row.mThumbnail;
```
Adapter类（从模型层获取数据，提供给RecyclerView显示）:   
预先：创造单例用于保存数据和传递数据   
1、RecyclerView布局文件名fragment_crime_list.xml，RecyclerView的id为crime_recycler_view    
```JAVA
ublic class CrimeListFragment extends Fragment {    
	private RecyclerView mCrimeRecyclerView;    
	@Override    
	public View onCreateView(LayoutInflater inflater,  ViewGroup container, Bundle savedInstanceState) {        
		View view=inflater.inflate(R.layout.fragment_crime_list,container,false);        
		mCrimeRecyclerView=(RecyclerView) view.findViewById(R.id.crime_recycler_view);        
		mCrimeRecyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));  //RecyclerView视图创建完成转交给LayoutManager对象        
		return view;    
	}
```
3、创建列表项视图list_item_crime.xml   
```XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"    
android:orientation="vertical"    
android:layout_width="match_parent"    
android:layout_height="wrap_content"    
android:padding="8dp">    
<TextView        
android:layout_width="match_parent"        
android:layout_height="wrap_content"        
android:text="Crime Title"/>    
<TextView        
android:layout_width="match_parent"        
android:layout_height="wrap_content"        
android:id="@+id/crime_date"        
android:text="Crime date"/>
</LinearLayout>
```
4、在CrimeListFragment.java中定义ViewHolder内部类，实例化并使用list_item_crime.xml布局   
```JAVA
private class CrimeHolder extends RecyclerView.ViewHolder{    
	public CrimeHolder(LayoutInflater inflater,ViewGroup parent){        
		super(inflater.inflate(R.layout.list_item_crime,parent,false));    
		}
	}
```
5、在CrimeListFragment.java里创建Adapter内部类   
```JAVA
private class CrimeAdapter extends RecyclerView.Adapter<CrimeHolder>{    
	private List<Crime> mCrimes;    
	public CrimeAdapter(List<Crime> crimes){        
		mCrimes=crimes;    
		}   
		@Override    
		public CrimeHolder onCreateViewHolder(ViewGroup parent, int viewType) {  //需要新的ViewHolder显示列表项时，调用该方法用它创建CrimeHolder        
			LayoutInflater layoutInflater=LayoutInflater.from(getActivity());        
			return new CrimeHolder(layoutInflater,parent);    
			}    
			@Override    
			public void onBindViewHolder(CrimeHolder holder, int position) {    
				}    
				@Override   
				public int getItemCount() {        
					return mCrimes.size();    
					}
				}
```
6、创建updateUI(）方法，该方法创建CrimeAdapter，设置给RecyclerView   
```JAVA
public class CrimeListFragment extends Fragment {    
	private RecyclerView mCrimeRecyclerView;    
	private CrimeAdapter mAdapter;    
	@Override    
	public View onCreateView(LayoutInflater inflater,  ViewGroup container, Bundle savedInstanceState) {        
		View view=inflater.inflate(R.layout.fragment_crime_list,container,false);        
		mCrimeRecyclerView=(RecyclerView) view.findViewById(R.id.crime_recycler_view);        
		mCrimeRecyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));        
		updateUI();    //        
		return view;    
	}
```
```JAVA
private void updateUI() {    
	CrimeLab crimeLab=CrimeLab.get(getActivity());    
	List<Crime> crimes=crimeLab.getCrimes();    
	mAdapter=new CrimeAdapter(crimes);    
	mCrimeRecyclerView.setAdapter(mAdapter);
}
```
7、绑定列表项，将CrimeListFragment.java的CrimeHolder类的构造方法内实例化视图组件   
```JAVA
rivate class CrimeHolder extends RecyclerView.ViewHolder{    
	private TextView mTitleTextView;    
	private TextView mDateTextView;   
	private Crime mCrime;    
	public CrimeHolder(LayoutInflater inflater,ViewGroup parent){        
		super(inflater.inflate(R.layout.list_item_crime,parent,false));        
		mTitleTextView=(TextView)itemView.findViewById(R.id.crime_title);       
		mDateTextView=(TextView)itemView.findViewById(R.id.crime_date);    
	}
```
8、在CrimeHolder类内实现bind(Crime)方法，取到一个Crime，CrimeHolder刷新显示TextView标题视图和日期视图  
```JAVA
rivate class CrimeHolder extends RecyclerView.ViewHolder{    
	private TextView mTitleTextView;    
	private TextView mDateTextView;    
	private Crime mCrime;    
	public CrimeHolder(LayoutInflater inflater,ViewGroup parent){        
		super(inflater.inflate(R.layout.list_item_crime,parent,false));        
		mTitleTextView=(TextView)itemView.findViewById(R.id.crime_title);        
		mDateTextView=(TextView)itemView.findViewById(R.id.crime_date);    
		}    
		private void bind(Crime crime){        
			mCrime=crime;        
			mTitleTextView.setText(mCrime.getTitle());        
			mDateTextView.setText(mCrime.getDate().toString());    
			}
		}
```
9、CrimeListFragment.java内的CrimeAdapter类的onBindViewHolder方法使用bind(Crime)方法   
```JAVA
public void onBindViewHolder(CrimeHolder holder, int position) {    
	Crime crime=mCrimes.get(position);    
	holder.bind(crime);
}
```
10、处理点击事件   
```JAVA
private class CrimeHolder extends RecyclerView.ViewHolder implements View.OnClickListener{
public CrimeHolder(LayoutInflater inflater,ViewGroup parent){    
	super(inflater.inflate(R.layout.list_item_crime,parent,false));   
	itemView.setOnClickListener(this);   //    
	mTitleTextView=(TextView)itemView.findViewById(R.id.crime_title);    
	mDateTextView=(TextView)itemView.findViewById(R.id.crime_date);
}
public void onClick(View view) {    
	Toast.makeText(getActivity(),mCrime.getTitle()+"clicked!",Toast.LENGTH_SHORT).show();
}
```
**Markdown**
*Awesome*