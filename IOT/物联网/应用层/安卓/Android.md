
## Activity

### 生命周期

#### 返回栈

​	Android是使用任务返回栈来管理Activity,一个任务就是一组存在栈中的Activity,在默认情况下,每次启动一个新的Activity,就会在返回栈中入栈,并处于栈顶的位置,每当按back键或调用finish()方法销毁一个Activity时,处于栈顶的Activity就会出栈,前一个入栈的Activity就会重新处于栈顶的位置,系统总是会显示处于栈顶的Activity给用户

#### Activity状态

1. 运行状态:当一个Activity位于返回栈的栈顶时,Activity处于运行状态。
2. 暂停状态:当一个Activity不再位于栈顶,但仍然可见时。
3. 停止状态:当一个Activity不再处于栈顶,并且完全不可见。
4. 销毁状态:一个Activity从返回栈中移除后。

#### Activity的回调方法

* onCreate():在Activity第一次被创建时调用,完成初始化操作,例如加载布局,绑定事件等等
* onStart():在Activity由不可见变为可见的时候调用
* onResume():在Activity准备好与用户进行交互的时候调用,此时的Activity一定位于返回栈的栈顶,并且处于运行状态
* onPause():在Activity准备去启动或者恢复另一个Activity的时候调用,通常在这个方法中将一些小号的CPU资源释放掉,以及保存一些关键数据
* onStop():在Activity完全不可见的时候调用
* onDestory():在Activity被销毁之前调用
* onRestart():在Activity由停止状态变为运行状态之前调用

#### Activity的生存期

* 完整生存期:Activity在onCreate()方法和onDestory()方法之间,进行初始化和释放内存的操作
* 可见生存期:Activity在onStart()方法和onStop()方法之间,在此期间,Activity对用户总是可见的,可能用户无法交互,可以进行资源的加载和释放
* 前台生存期:Activity在onResume()方法和onPause()方法之间,Activity总是处于运行状态,也是可以和用户进行交互的

#### Activity的回收机制

​	Activity提供了一个onSaveInstanceState()回调方法,这个方法保证在Activity被回收之前一定会被调用,onSaveInstanceState()方法携带一个Bundle类型的参数,Bundle提供了一些列的方法用于保存数据,比如使用putString()方法保存字符串,使用putInt()方法保存整数型数据,每个保存方法需要传入两个参数,第一个参数是键,用于后面从Bundle中取值,第二个参数是真正要保存的内容

```kotlin
override fun onSaveInstanceState(outState:Bundle){
	super.onSaveInstanceState(outState)
	val tempData = "..."
	outState.putString("data_key",tempData)
}

// 恢复
override fun onSaveInstanceState(outState:Bundle?){
	...
    if(savedInstanceState != null){
        val tempdata = savedInstanceState.getString("data_key")
        
    }
}
```

#### Activity的启动模式

​	Activity的启动模式一共有4种,分别是standard,singleTop,singleTask和singleInstance,可以在AndroidManifest.xml中通过给<activity>标签指定android:launchMode属性来选择启动模式

* standard

​	standard是默认的启动模式,在此模式下,每当启动一个新的Activity,它就会在返回栈中入栈,并处于栈顶的位置,对于使用standard模式的Activity,系统不会在乎Activity是否已经在返回栈中存在,每次启动都会创建一个该Activity的新实例。

* singleTop

​	singleTop模式在启动Activity时,如果发现返回栈的栈顶已经是该Activity,则认为可以直接使用它,不会再创建新的Activity实例

* singleTask

​	singleTask模式启动Activity时,系统首先会检查返回栈中是否存在该Activity的实例,如果发现已经存在则直接使用该实例,并把在这个Activity之上的所有其他Activity统统出栈,如果没有就会创建一个新的Activity实例。

* singleInstance

​	singleInstance模式下启动Activity,系统会创建一个新的返回栈管理它。



### 启动

- 登录启动:即在程序开始时就启动该 Activity

```xml
<activity
    android:name=".MainActivity
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
</activity>
```

- 默认启动:需要借助 Intent 启动 Activity

​	**android:exported="true"**必须加入,代表该activity可以被其他activity启动

```xml
<activity
    android:name=".MainActivity
    android:exported="true">
    <intent-filter>
        <action android:name="com.example.activitytest.ACION_STSRT"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="com.example.activitytest.MY_CATEGORY"/>
    </intent-filter>
</activity>
```

- Intent 有两种启动方式:显式启动和隐式启动  
  显式启动需要提供启动 Activity 的上下文 Context 和需要启动的目标 Activity,然后通过 startActivity(intent)函数启动

```kotlin
val intent = Intent(this,SecondActivity::class.java)
startActivity(intent)
```

​	隐式启动需要额外加入 category,提前在 xml 的 category 标签加入,android.intent.category.DEFAULT,为默认 category,可以不加

```kotlin
val intent = Intent("com.example.activitytest.DEFAULT")
intent.addCategory("com.example.activitytest.MY_CATEGORY")
startActivity(intent)
```

* 写一个好的启动方式

```kotlin
class SecondActivity:BaseActivity(){
    companion object{
        fun actionStart(context:Context,data1:String,data2:String){
            val intent = Intent(context,SecondActivity::class.java)
            intent.putExtra("param1",data1)
            intent.putExtra("param2",data2)
            context.startActivity(intent)
        }
    }
}


// 
SecondActivity.actionStart(this,"data1","data2")
```



### 视图绑定

​	此方法用于代替旧版本findViewById()和kotlin-android-extensions插件无法使用的问题,要进行视图绑定需要在项目级build.gradle中加入BuildFeature启动Binding

```kotlin
android{
    ...
    buildFeatures{
        viewBinding = true
    }
}    
```



​	如需设置绑定类的实例以供 Activity 使用，请在 Activity 的 onCreate() 方法中执行以下步骤：

- 调用生成的绑定类中包含的静态 inflate() 方法。此操作会创建该绑定类的实例以供 Activity 使用。
- 通过调用 getRoot() 方法或使用 Kotlin 属性语法获取对根视图的引用。
- 将根视图传递到 setContentView()，使其成为屏幕上的活动视图。

* 由于 inflate(layoutInfalter)属于弱绑定,所以要 setContentView(binding.root)进行布局绑定

```kotlin
private lateinit var binding:ActivityTestBinding
    override fun onCreate(saveInstanceState: Bundle?){
        super.onCreate(saveInstanceState)
        if(!::binding.isInitialized){
            binding = ActivityTestBinding.inflate(layoutInflater)
        }
        val view = binding.root
        setContentView(view)
    }
```

​	在进行了视图绑定之后,可以根据binding来获取xml中控件的id

```kotlin
binding.button.setOnClickListenner(){
	...
}
```



### 集中管理

​	建立一个单例类集中所有的Activity进行管理

```kotlin
object ActivityCollector{
	private val activities = ArrayList<Activity>()
	
	fun addActivity(activity:Activity){
		activities.add(activity)
	}
	
	fun removeActivity(activity:Activity){
        activities.remove(activity)
    }
    
    fun finishAll(){
        for(activity in activities){
            if(!activity.isFinishing){
                activity.finish()
            }
        }
        activities.clear()
        activities.resize() // 重置数组大小,防止内存泄漏
        android.os.Process.killProcess(android.os.Process.myPid()) // 用于删除当前程序的进程
    }
	
}
```

​	每一个Activity都可以使用其对自身进行管理

```kotlin
class ThirdActivity:BaseActivity(){
    override fun onCreate(savedInstanceState:Bundle?){
        ...
        ActivityCollector.addActivity(this)
    }
    
    override fun onDestroy(){
        super.onDestory()
        ActivityCollector.removeActivity(this)
    }
}
```

​	想要退出全部程序

```kotlin
ActivityCollector.finishAll()
```



## Fragment

1.视图绑定

- 如需设置绑定类的实例以供 Fragment 使用，请在 Fragment 的 onCreateView() 方法中执行以下步骤：

- 调用生成的绑定类中包含的静态 inflate() 方法。此操作会创建该绑定类的实例以供 Fragment 使用。

- 通过调用 getRoot() 方法或使用 Kotlin 属性语法获取对根视图的引用。
  从 onCreateView() 方法返回根视图，使其成为屏幕上的活动视图。
* 还可以在***onViewCreated***中实现各种功能,此函数在fragment由可见到不可见时会调用
```kotlin
 private lateinit var binding: ResultProfileBinding
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
            if(!::binding.isInitialized){
                binding = ResultProfileBinding.inflate(inflater, container, false)
        }
        val view = binding.root
        return view
    }
```

2.动态加载

- 现在 xml 文件里面准备好 FrameLayout 布局作为容器

```xml
<FrameLayout>
    android:id=".."
    android:layout_width="0dp"
    androuid:layout_height="match_parent"
    android:layout_weight="1">
</FrameLayout>
```

- 创建待添加 Fragment 实例
- 获取 FragmentManager,在 Activity 中调用 getSupportFragmentManager()获取
- 开启一个事务,通过 beginTransaction()方法开启
- 想容器类添加或替换 Fragment,使用 repalce()方法实现,需要传入容器 id 和待添加的 Fragment 实例
- 提交事务,调用 commit()方法完成

```kotlin
fun replaceFragment(layoutId:Int,fragment:Fragment){
    val fragmentManger = supportFragmentManager
    val transaction = fragmentManger.beginTransaction()
    transaction.replace(layoutId,fragment)
    transaction.commit()
}
```

3. onAttach

- onAttach 方法是 Fragment 生命周期中的一个回调方法，在 Fragment 被附加到宿主 Activity 时调用。你可以在 onAttach 方法中获取宿主 Activity 的引用并进行一些初始化操作。

```kotlin
override fun onAttach(context: Context) {
        super.onAttach(context)

        // context is the reference to the hosting Activity
        // You can typecast it to your MainActivity or any other specific Activity if needed
        if (context is MainActivity) {
            val mainActivity = requireContext() as MainActivity
            // Now you can interact with MainActivity and call its methods
        }
    }
```

4. requireContext() 可以在fragment创建视图时获取父类activity的context

```kotlin
override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        if(!::binding.isInitialized){
            binding = FragmentConnectBinding.inflate(inflater, container, false)
        }
        // 获取父类的context
        if (!::mainActivity.isInitialized){
            mainActivity = requireActivity() as MainActivity
        }
        // 创建mqttClienthelper实例
        mqttClienthelper = mqttClientHelper(mainActivity)

    }
```





## Intent

​	Intent是Android程序中各组件之间进行交互的一种重要方式,它不仅可以知明当前组件想要执行的动作,还可以在不同组件之间传递数据。Intent一般还可用于启动Activity,启动Service已经发送广播等场景。

​	Intent大致分两种,显式Intent和隐式Intent。

### 显式Intent

​	构造函数,Intent(Context packageContext,Class<?> cls),这个构造函数接收两个参数:第一个参数Context要求提供要给启动Activity的上下文,第二个参数Class用于指定想要启动的目标Activity,使用Activity的startActivity()方法就可以借助Intent启动Activity

```kotlin
val intent = Intent(this,SecondActivity::class.java)
startActuvuty(intent)
```

### 隐式Intent

​	相较于显式Intent,隐式Intent并不明确指明要启动哪一个Activity,而是指定了一系列更为抽象的action和category等信息,然后交由系统去分析这个Intent,并帮我们找出合适的Activity去启动

​	使用隐式Intent必须要在注册表中activity的<action>和<category>这两个内容同时匹配才能响应,在startActivity中也不需要传入上下文

```xml
<activity
    android:name=".MainActivity
    android:exported="true">
    <intent-filter>
        <action android:name="com.example.activitytest.ACION_STSRT"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="com.example.activitytest.MY_CATEGORY"/>
    </intent-filter>
</activity>
```

```kotlin
val intent = Intent("com.example.activitytest.ACION_STSRT")
intent.addCategory("android.intent.category.DEFAULT") // DEFAULT 可省略
startActivity(intent)
```

​	**android.intent.category.DEFAULT是一种默认的category,不加入category也能启动**

**隐式Intent调用系统程序**

​	基本格式

```kotlin
val intent = Intent(action)
intent.data = Uri.prase(Uri)
```

​	intent.data 实际上是调用了Intent的setData()方法的语法糖解析传入的Uri

* 网页

action 为 Intent.ACTION_VIEW,Uri为对应的网址

```kotlin
val intent = Intent(Intent.ACTION_VIEW)
intent.data = Uri.prase("https://www.baidu.com")
startActivity(intent)
```

* 电话

action 为Intent.ACTION＿DIAL,Uri为电话号码

```kotlin
val intent = Intent(Intent.ACTION_DIAL)
intent.data = Uri.prase("tel:10086")
startActivity(intent)
```

使用Intent传递信息

* 向下一个Activity传递数据

```kotlin
val data = "Hello SecondActivity"
val Intent = Intent(this,SecondActivity:class.java)
intent.putExtra("extra_data",data)
startActivity(intent)
```

​	接收数据

```kotlin
class SecondActivity:AppCompatActivity(){
	override fun onCreate(savedInstanceState:Bundle?){
		...
		val extraData = intent.getStringExtra("extra_data") // getIntExtra() getBooleanExtra() ...
	}
}
```

* 返回数据给上一个Activity

​	原先的startActivityForResult方法已经废除,现在使用登陆器的方式实现向上传递参数,首先在第一个activity中请求一个登陆器

```kotlin
private val requestDataLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()){result-> when(result.resultCode){RESULT_OK -> result.data?.getStringExtra( "data")}
```

​	借助登陆器启动第二个activity

```kotlin
val intent = Intent(this,SecondActivity::class.java)
requestDataLauncher.launch(intent)
```

​	第二个activity则需要定义一个 Intent 当作容器来传递信息,然后用 setResult()方法传递处理结果和 intent,注意setResult的result值需要跟登陆器的result值相等

```kotlin
val intent = Intent()
intent.putExtra("data",data)
setResult(RESULT_OK,intent)
finish()
```





## 交互

1.Activity 和 Activity 之间的交互

- 向下传递,使用 putExtra()方法传递,第一个参数为健,第二个参数为值

```kotlin
val intent = Intent(this,SecondActivity::class.java)
intent.putExtra("extra_data",data)
startActivity(intent)
```

- 接收 intent 的类会调用父类的 getIntent()方法,调用 getStringExtra()方法获得字符串

```kotlin
val extraData = intent.getStringExtra("extra_data")
```

- 向上传递
- 首先要注册请求登陆器

```kotlin
private val requestDataLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()){result-> when(result.resultCode){RESULT_OK -> result.data?.getStringExtra( "data")}
```

- 然后借助登陆器启动 intent

```kotlin
val intent = Intent(this,SecondActivity::class.java)
requestDataLauncher.launch(intent)
```

- SecondActivity 则需要定义一个 Intent 当作容器来传递信息,然后用 setResult()方法传递处理结果和 intent

```kotlin
val intent = Intent()
intent.putExtra("data",data)
setResult(RESULT_OK,intent)
finish()
```

2. activity 和 fragment,fragment 和 fragment 之间的交互

- 都是通过类型的转换进行交互,fragment 的 getActivity()方法会自动获取与之关联的 activity

```kotlin
val mianAvctivity = activity as? MainActivity
```

- 也可以使用 requireContext()函数来避免空检测

```
val mainActivity = requireContext() as MainActivity
```

- 之后便可以利用 mainActivity 调用 Activity 的方法了
- 而 fragment 和 fragment 的交互也可以通过 Activity 作为媒介进行交互

## 布局

### 线性布局

​	线性布局LinearLayout又称为线性布局,该布局会将它所包含的控件在线性方向上依次排列。

* **_orientation_**

​	指定子控件的排列方向

```xml
<LinearLayout
    ...
    android:orentation = "vertical" 垂直方向
    android:orentation = "horizontal" 水平方向>
</LinearLayout>
```

* **_gravity_**

​	用于指定文字在控件中的对齐方式

```xml
android:gravity = "top" 顶部
android:gravity = "center_vertical" 垂直居中
android:gravity = "bottom" 底部
```

* **_layout_gravity_**

​	指定控件相对于父控件的位置和控件内容相对于控件的位置

```xml
android:layout_gravity = "top" 顶部
android:layout_gravity = "center_vertical" 垂直居中
android:layout_gravity = "bottom" 底部
```

* **_layout_width_**

​	可以通过比例的方式指定控件的大小

```xml
<EditText
	...
	android:layout_width = "0dp"
	android:layout_weight = "1" 宽度按照1比1分割
/>

<Button
	...
	android:layout_width = "0dp"
	android:layout_weight = "1" 宽度按照1比1分割
/>
```

### 相对布局

​	相对布局RelativeLayout通过指定相对定位的方式让控件出现在布局的任何位置。

````xml
<RelativeLayout>
	...
</RelativeLayout>
````



* **layout_alignParentLeft**,**layout_alignParentRight**,**layout_alignParentTop**,**layout_alignParentBottom**,**layout_centerInParent**分别指定该控件位于父布局的左边,右边,上边,下边,和中心位置

```xml
android:layout_alignParentLeft = "true"
android:layout_alignParentRight = "true"
android:layout_alignParentTop = "true"
android:layout_alignParentBottom = "true"
android:layout_centerInParent = "true"
```

* **layout_above**,**layout_below**,**layout_toLeftOf**,**layout_toRightOf**用于指定相对控件的位置

```xml
android:layout_above = "@+id/id1" 相对id1上面
android:layout_below = "@+id/id1" 相对id1下面
android:layout_toLeftOf = "@+id/id1" 相对id1左边
android:layout_toRightOf = "@+id/id1" 相对id1右边
```

* **layout_alignLeft**,**layout_alignRight**,**layout_alignTop**,**layout_alignBottom**用于指定边沿的对齐

### 帧布局

​	帧布局FrameLayout,默认所有控件摆放在布局的左上角

```xml
<FrameLayout>
	...
</FrameLayout>
```

​	控件可以用layout_gravity指定相对于FrameLayout的位置



## 控件

### Toast

​	Toast是Android系统提供的一种非常好的提醒方式,在程序中可以使用它将一些短小的信息通知给用户,这些信息会在一段时间后自动消失,并且不会占用任何屏幕空间。

​	Toast通过静态方法makeText()创建出一个Toast对象,然后调用show()将Toast显示出来,makeText()方法需要传入3个参数,第一个参数	Context,第二个参数是Toast显示的文本信息,第三个是Toast显示的时长,有两个内置常量可以选择:Toast.LENGTH_SHORT和Toast.LENGTH.LONG

```kotlin
Toast.makeText(this,"...",Toast.LENGTH_SHORT)
```



### Snackbar

​	Snackbar是Material库提供的更加先进的提醒工具,它允许在提示中加入一个可交互按钮,当用户点击按钮时,可以执行一些额外的逻辑操作。

```kotlin
Snackbar.make(view,"Data deleted",Snackbar.LENGTH_SHORT)
.setAction("Undo"){
    Toast.makeText(this,"Data restored ",Toast.LENGTH_SHORT).show()
}.show()
```

​	Snackbar的make()方法第一个参数需要传入一个View,Snackbar会使用这个View自动查找最外层的布局,用于展示提示信息,第二个参数就是Snackbar中显示的内容,第三个是Snackbar显示的时长。setAction()方法用于设置一个动作,用lambda函数写具体的事件,传入的参数为按钮的显示文字。



### RecyclerView

​	RecyclerView作为新增属性,需要在项目级的build.gradle中添加RecyclerView库的依赖,具体版本不定

```kotlin
implementation("androidx.recyclerview:recyclerview:1.0.0")
```

​	将控件加入到布局xml中

```xml
<androidx.recyclerview.widget.RecyclerView
	android:id="@+id/recyclerView"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
/>
```

​	接下来配置RecyclerView的三大基本组件:ListItem,Adapter和LayoutManager

1. ListItem

​	ListItem作为RecyclerView的子项,需要配置相应的类和布局

​	新建一个数据类

```kotlin
data class News(val title:String,val content:String)
```

​	在layout布局目录下新建xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/newsTile"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
</TextView>
```

2. LayoutManager

​	LayoutManager用作规定RecyclerView中ListItem的排列方式,有线性布局**LinearLayoutManager**,网格布局**GridLayoutManager**和瀑布流布局**StaggeredGridLayoutManager**

```kotlin
class NewsTitleFragment:Fragment() {
    private lateinit var binding:NewsTitleFragmentBinding

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        if(!::binding.isInitialized){
            binding = NewsTitleFragmentBinding.inflate(inflater,container,false)
        }
    	val layoutManager = LinearManager(this)
        // layoutManager.orientation = LinearLayoutManager.HORIZONTAL 水平布局
        // val layoutManager = StaggeredGridLayoutManager(3,StaggeredGridLayoutManager.VERTICAL) 3列,垂直布局
        binding.newsTitleRecyclerView.layoutManager = LinearLayoutManager(this)

        return binding.root
    }

}

```



3. Adapter

​	Adapter可以作为Activity或Fragment的内部类构建,用于绑定ListItem数据与视图,控制点击的逻辑事件

```kotlin
class NewsTitleFragment:Fragment() {
    private lateinit var binding:NewsTitleFragmentBinding

    /*
    	val adapter by lazy{
    		NewsAdapter(expressionList)
		}
	*/
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        if(!::binding.isInitialized){
            binding = NewsTitleFragmentBinding.inflate(inflater,container,false)
        }
        
        
        /*
            with(binding.recyclerView) {
        		layoutManager = LinearLayoutManager(this)
        		adapter = adapter
            }
        */
        val layoutManager = LinearLayoutManager(this)
        binding.newsTitleRecyclerView = layoutManager // 一定要在adapter赋值之前
        
        val adapter = NewsAdapter(getNews())
        binding.newsTitleRecyclerView.adapter = adapter
		
        // 高级写法
        with(binding.newsTitleRecyclerView){
            layoutManager = LinearManager(this@MainActivity)
        	adapter = adapter
        }
        
        return binding.root
    }

    // 适配器
    inner class NewsAdapter(val context: Context,val newsList:List<News>): RecyclerView.Adapter<NewsAdapter.ViewHolder>() {
            inner class ViewHolder(val binding: NewsItemBinding) : RecyclerView.ViewHolder(binding.root)

        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
            val binding: NewsItemBinding =
                NewsItemBinding.inflate(LayoutInflater.from(context), parent, false)
            val holder = ViewHolder(binding)
            return ViewHolder(binding)
        }

        override fun onBindViewHolder(holder: ViewHolder, position: Int) {
            val news = newsList[position]
            holder.binding.newsTile.text = news.title
            holder.itemView.setOnClickListener{
                ...
            }
        }

        override fun getItemCount() = newsList.size

    }

    private fun getNews():List<News>{
        val newsList = ArrayList<News>()wo
        for(i in 1..50){
            val news = News("This is news title $i",getRandomLengthString("This is news content $i"))
            newsList.add(news)
        }
        return newsList
    }

    private fun getRandomLengthString(str:String) = str * (1..20).random()
}

```

​	* Adapter的构造函数需要传入Context上下文和List,Adapter继承于RecyclerView.Adapter,RecyclerView.Adapter的模板是ViewHolder类型的

* ViewHolder继承于RecyclerView.ViewHolder,用于RecyclerView视图绑定的媒介,将ListItem的xml与RecyclerView进进行绑定,RecyclerView.ViewHolder的构造函数需要传入子类布局的view参数,即binding.root
* Adaper需要重写onCreateViewHolder(),onBindViewHolder(),getItemCount()这三个方法
* onCreateViewHolder()用于创建ViewHolder实例,进行视图绑定, 子类binding需要通过inflate(LayoutInflater.from(parent.context), parent, false)函数获取
* onBindViewHolder()能够进行数据处理,对ListItem的数据进行赋值,会在每个子项被滚动到屏幕类的时候执行,可以通过position获取当前的ListItem实例,再通过binding获取ListItem的属性进行赋值,还可以对每一项进行具体的响应操作
* getItemCount()用于获得List的长度
* 最后创建这个adapter实例并赋值给Activity中RecyclerView的adapter	

## Broadcast

　	Broadcast分两种类型：标准广播和有序广播

* 标准广播：是一种完全异步执行的广播，在广播发出之后，所有的BroadcastReceiver几乎会同一时刻收到这条广播消息，因此它们之间没有任何先后顺序可言。这种广播的效率会比较高，这也意味着它是无法被截断的。

* 有序广播：是一种同步执行的广播。在广播发出之后，同一时刻只会有一个BroadcastReceiver能够收到这条消息，当这个BroadcastReceiver中的逻辑执行完毕后，广播才会继续传递。所以此时的BraadcastReceiver是有先后顺序的，优先级高的BroadcastReceiver就可以收到广播消息，并且前面的BroadcastReceiver还可以截断正在传递的广播，这样后面的BroadcastReceiver就无法收到广播的消息了。

* BroadcastReceiver类用于接收和处理广播

  ```kotlin
  class MyBoardcastReceiver:BroadcastReceiver(){
      override fun onReceive(context:Context,intent:Intent){
          ...
      }
  }
  ```

  ​	MyBoardcastReceiver类继承BroadcastReceiver，并且重写了父类的onReceiver（）方法，每当收到广播时，onReceive内的函数都会执行

###  自定义广播

### 系统广播

* 动态注册一个系统广播：

​		使用系统广播，可以在<Android SDK>/platforms/<android api>/data/brocast_actions.txt中查看完整的系统广播列表,android.intent.action.TIME_TICK这个系统广播会在1分钟发送一次，通过IntentFilter实例添加这个action，最后传入到registerReceiver之后就完成了系统广播和BroadcastReceiver的绑定。另外一定要在Activity销毁之前取消掉他们之间的绑定。

```kotlin
class MainActivity:AppCompatActivity(){
	private lateinit var myBoardcastReceiver:MyBoardcastReceiver
	override fun onCreate(savedInstanceState:Bundle?){
		...
        val intentFilter = IntentFilter()
        intentFilter.addAction("android.intent.action.TIME_TICK")
        myBoardcastReceiver = MyBoardcastReceiver()
        registerReceiver(myBoardcastReceiver,intentFilter) //注册广播
	}
    
    override fun onDestory(){
        super.onDestory()
        unregisterReceiver(myBoardcastReceiver) // 取消注册
    }
}
```

* 静态注册一个系统广播：

​	静态注册方法的好处就是可以在系统未启动的情况下也能接收广播。静态注册中的BroadcastReceiver一定要在AndroidManifest.xml中注册才可以使用。可以用Android Studio的快捷方式创建BroadcastReceiver，自动完成注册。在<intent-filter>中加入android.intent.action.BOOT_COMPLETED的action完成静态注册 ，另外需要给这个action添加权限

```xml
<uses-Permission android:name = "android.permission.RECEIVE_BOOT_COMPLETED"
<application>
	<receiver
	android:name = ".myBoardcastReceiver"
	android:enabled = "true"
    android:exported = "true">
       <intent-filter>
            <action android:name = "android.intent.action.BOOT_COMPLETED"/>
        </intent-filter>
	</receiver>

</application>

```

Exported属性表示是否允许这个BroadcastReceiver接收本程序以外的广播，Enabled属性表示是否启用这个BroadcastReceiver。 

#### 标准广播

1. 注册一个BroadcastReceiver,加入自定义的action com.example.broadcasttext.MY_BOARDCAST

```xml
<application>
	<receiver
	android:name = ".myBoardcastReceiver"
	android:enabled = "true"
    android:exported = "true">
       <intent-filter>
            <action android:name = "com.example.broadcasttext.MY_BOARDCAST"/>
        </intent-filter>
	</receiver>

</application>
```

- 用Intent对象作为自定义标准广播的载体,将要发送的广播的值传入，调用Intent的setPackage() 方法，并传入当前应用程序的包名，packageName是getPackageName()的语法糖写法，用于获取当前应用程序的包名，最后用sendBroadcast()方法将广播发送出去，这样监听这条广播的BroadcastReceiver就会收到消息了。

```kotlin
val intent = Intent("com.example.broadcasttext.MY_BOARDCAST")
intent.setPackage(packageNume)
sendBroadcast(intent)
```

#### 有序广播

​	通过priority给多个BroadcastReceiver设置优先级

```xml
<receiver
    android:name=".MyBroadcastReceiver"
    android:enabled="true"
    android:exported="true">
    <intent-filter android:priority="100">
        <action android:name="com.example.broadcasttest.MY_BROADCAST" />
    </intent-filter>
</receiver>
```

​	用sendOrderedBroadcast(intent,null)发送有序广播,sendOrderedBroadcast()方法接收两个参数：第一个参数是Intent，第二个参数是一个与权限相关的字符串，可以直接传入null

```kotlin
val intent = Intent("com.example.broadcasttext.MY_BOARDCAST")
intent.setPackage(packageNume)
sendOrderedBroadcast(intent,null)
```

​	优先级高的BroadcastReceiver可以用abortBroadcastr()函数将广播截断

```kotlin
class MyBoardcastReceiver:BroadcastReceiver(){
    override fun onReceive(context:Context,intent:Intent){
		abortBroadcastr()
    }
}
```



## 持久性存储

### 文件存储

#### 将数据存储到文件中

* Context类中提供**openFileOutput()**方法,用于将数据存储到指定文件中。这个方法接收两个参数:第一个是文件名,在文件创建时使用,文件名不可包含路径,文件默认存储在**/data/data/<package name>/file/**目录下,第二个参数是文件的操作模式,主要有**MODE_PRIVATE**和**MODE_APPEND**两种模式可选,默认是MODE_PRIVATE,表示当指定相同文件名的时候,说写入的内容将会覆盖原文件中的内容,而MODE_APPEND表示如果该文件已存在,就往文件里面追加内容,不存在就创建新文件。

```kotlin
fun save(inputText:String){
	try{
		val output = openFileOutput("data",Context,MODE_PRIVATE)
		val writer = BufferedWeiter(OutputStreamWriter(output))
		writer.use{
			it.write(inputText)
		}
	}catch(e:IOException){
		e.printStackTrace()
	}
}
```

* 通过**openFileOutput()**方法得到一个**FileOutputStream**对象,根据它构建出一个**OutputStreamWriter**对象,接着再使用**OutputStreamWriter**构建出一个**BufferedWriter**对象,通过**BufferedWriter**将文本内容写入文件中
* 这里使用了use函数,保证文件自动关闭

#### 从文件读取数据

* Context类提供**openFileInput()**方法用于从文件中来读取数据,只接受一个参数,即要读取的文件名,然后系统会自动到文件目录下加载这个文件,并返回一个FIleInputStream对象,得到这个对象之后,再通过流的方式就可以将数据读取出来。

```
fun load():String{
	val content = StringBuilder()
	try{
		val input = openFileInput("data")
		val reader = BufferedReader(InputStreamReader(input))
		reader.use{
			reader.forEachLine{
				content.append(it)
			}
		}
	}catch(e:IOException){
		e.printStackTrace()
	}
	return content.toString()

}
```

* 首先通过openFileInput()方法获取了一个**FileInputStream**对象，然后 借助它又构建出了一个**InputStreamReader**对象，接着再使用**InputStreamReader**构建出 一个**BufferedReader**对象，这样我们就可以通过**BufferedReader**将文件中的数据一行行读 取出来，并拼接到**StringBuilder**对象当中，最后将读取的内容返回
* 这里从文件中读取数据使用了一个forEachLine函数，这也是Kotlin提供的一个内置扩 展函数，它会将读到的每行内容都回调到Lambda表达式中



### SharedPreferences存储

​	SharedPreferences是使用键值对的方式来存储数据的。当保存一条数据的时候，需要给这条数据提供要给对应的键，读取数据的时候通过这个键把相应的值取出来。

#### 创建一个SharedPreferences对象

​	在Context类中创建：使用getSharedPreferences()方法，此方法接收两个参数，第一个参数用于指定SharedPreferences文件的名称，如果指定的文件不存在则会创建一个，存放在/data/data/<package name>/shared_prefs/目录下，第二个参数用于指定操作模式，目前只能选择MODE_PRIVATE。

​	在Activity类中创建：使用getPreferences()方法，只接收一个操作模式的参数，使用这个方法时会自动将当前Activity的类名作为SharedPreferences的文件名。

#### 存储数据

​	向SharedPreferences存储数据需要三个步骤：

1. 调用SharedPreferences对象的edit()方法获取一个SharedPreferences.Editor对象。

2. 向SharedPreferences.Editor对象中添加数据。
3. 调用apply（）方法将添加的数据提交，从而完成数据存储操作。

```kotlin
val editor = getSharePreferences("data",Context.MODE_PRIVATE).edit()
editor.putString("name","Tom")
editor.putInt("age",28)
editor.putBoolean("married",false)
editor.apply()
```

#### 读取数据

​	SharedPreferences对象提供了一系列的get()方法，这些get方法接收两个参数，第一个参数是键，传入存储数据时使用的键就可以得到相应的键了，第二个参数是默认值，即表示当传入的键找不到对应的值时会以默认值返回。

```kotlin
val prefs = getSharePreferences("data",Context.MODE_PRIVATE)
val name = prefs.getString("name","")
val age = prefs.getInt("age",0)
val married = prefs.getBoolean("married",false)
```



### 内置的 SQLite 数据库

#### 创建数据库 

- 使用 **SQLiteOpenHelper** 类,需要指定类继承,并重写 onCreate()和 onUpgrade()方法 ,需要传入4个参数：第一个参数为Context，第二个参数是数据库名，创建数据库时使用的就是这个数据库名，第三个参数允许我们在查询数据时返回一个自定义的Cursor，一般传入null即可，第四个参数是当前数据库的版本号，用于对数据库进行升级操作。
  

```kotlin
class MyDatabaseHelper(val context:Context,name:String,version:Int):SQLiteOpenHelper(context,name,null,version) {

    private val createBook = "create table Book(" +
            "id integer primary key autoincrement," + //autoincrement代表自增长
            "author text," +
            "price real," +
            "pages integer," +
            "name text)"

    override fun onCreate(db: SQLiteDatabase?) {
		db.exeecSQL(createBook)
    }

    override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
		
    }

}

```

* 实例化**SQLiteOpenHelper**

​	SQLiteOpenHelper使用getReadableDatabase()和getWriteableDatabase()方法创建或打开一个现有的数据库（如果数据库已存在则直接打开 ，否则要创建一个新的数据库），并返回一个可对数据库进行读写操作对象。不同的是当数据库不可写入时（如磁盘空间已满），getReadableDatabase() 方法返回的对象将以只读的方式打开数据库，而getWriteableDatabase()方法则将出现异常。构建**SQLiteOpenHelper**实例后，再调用它的getReadableDatabase()或getWriteableDatabase()方法就能创建数据库，此时重写的onCreate()方法也会得到执行。数据库文件会存放再/data/data/<package name>/databases/目录下。

```kotlin
val dpHelper = MyDatabaseHelper(this,"BookStore.db",1)
dpHelper.writeableDatabase // getWriteableDatabase()语法糖
dpHelper.readableDatebase // getReadableDatabase()语法糖
```

#### CRUD 操作

- ContentValues

​	ContentValues是QSlit数据库的存储容器，提供了一系列的put()方法重载，用于向ContentValues()中添加数据，可以将数据组装成Table中的一行。

```kotlin
val values1 = ContentValues().apply {
                put("name","The Da Vinci Code")
                put("author","Dan Brown")
                put("pages",454)
                put("price",16.96)
            }

// 进阶 使用内置封装函数
val values = contentValuesOf("name" to "The Da Vinci Code","author" to "Dan Brown"，"pages" to 454,"price" to 16.96)
```

##### 添加数据  
​	使用insert()方法，用于添加数据。insert()方法接收三个参数:第一个参数为表名,第二个参数用于在未指定添加数据的情况下给某些可为空的列自动赋值，填入null即可,第三个参数为 ContentValues 对象

```kotlin
db.insert("Book",null,values1)
```

##### 删除数据  
​	使用delete()方法用于删除数据。delete()方法接收三个参数:第一个参数为表名,第二第三参数用于约束删除某一行或者某几行的数据，不指定就会默认删除所有行。

```kotlin
db.delete("Book","pages > ?", arrayOf("500"))
```

##### 查询数据

​	使用query()用于对数据进行查询。调用此query()方法最终会返回一个 cursor 对象,利用其进行取值操作,最后要用closte()方法关闭cursor 对象。以下是传入参数的列表。

|  query()参数  |      对应 SQL 部分       |               描述               |
| :-----------: | :----------------------: | :------------------------------: |
|     table     |     from table_name      |          指定查询的表名          |
|    columns    |  select column1,column2  |          指定查询的列名          |
|   selection   |   where column = value   |       指定 where 约束条件        |
| selectionArgs |            -             | 为 where 中的占位符提供具体的值  |
|    groupBy    |     group by column      |      指定需要 group by 的列      |
|    having     |  having column = value   | 对 group by 后的结果进一步的约束 |
|    orderBy    | order by column1,column2 |      指定查询结果的排序方式      |



```kotlin
val cursor = db.query("Book",null,null,null,null,null,null)
if(cursor.moveToFirst()){ // moveToFirst()方法将数据的指针移动到第一行的位置
    do {
            // 遍历Bookl表中的所有数据
            val name = cursor.getString(cursor.getColumnIndexOrThrow("name")) // 新版：使用getColumnIndexOrThrow获得列序列的索引，然后使用get...方法取出值
            val author = cursor.getString(cursor.getColumnIndexOrThrow("author"))

        }while (cursor.moveToNext())
    }
    cursor.close()
}
```

##### 更新数据  
​	使用update()方法用于数据更新。这个方法接收四个参数:第一个参数为表名,第二个参数为 Contentvalues 对象,第三第四参数用于约束某一行或某几行中的数据，不指定的话会默认更新所有行。

```kotlin
db.update("Book",values,"name = ?", arrayOf("The Da Vinci Code"))
```



## Jetpack

​	Jetpack是一个开发组件工具集，可以简化组件的开发过程，通常第一在AndroidX库中，有着很好的向下兼容性。

### ViewModel

​	**_ViewModel_**专门用于存放与界面相关的数据,它的生命周期在Acitvity的整个生命周期内都是存在的，可以保证手机在发生旋转时不会被重写创建，只有当Activity退出时，才会跟着Activity一起销毁。

​	在使用ViewModel组件之前要引入依赖

```kotlin
implementation("androidx.lifecycle:lifecycle-extensions:2.2.0")
```

- 创建**_ViewModel_**类

```kotlin
class MainViewModel:ViewModel() {
    var counter = 0
}
```

- 获取ViewModel实例

​		不能以构造函数的方式直接创建ViewModel的实例，要通过ViewModelProvider的方法获取ViewModel实例，这是因为ViewModel有其独立的生命周期，并且其生命周期要长于Activity。

```kotlin
val viewModel = ViewModelProvider(this).get(MainViewModel::class.java)

```

​	更加方便的viewModel实例化方法，通过by关键字创建,可以直接创建viewModel,与正常实例化的viewModel相比,生命周期绑定与activity或者fragment,自动管理生命周期

```kotlin
 private val viewModel : MainViewModel by viewModels()
```

### Lifecycles

​	Lifecycles 用于监视 Activity 或者 Fragment 的生命周期

​	首先要实现LifecycleObserver空接口,在内部函数上使用了@OnLifecycleEvent注解，并传入生命周期事件,一共有7种:ON_CREATE,ON_START,ON_RESUME,ON_PAUSE,ON_STOP和ON_DESTROY分别匹配Activity中相应的生命周期回调，另外的ON_ ANY类型则可以匹配任何的生命周期回调。

```kotlin
class MyObserver:LifecycleObserver{
    
    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun activityStart(){
        ...
    }

	@OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun activityStop(){
        ...
    }
    
}


```

​	通过lifecycle的addObserver方法添加MyObserver类，这样MyObserver就能自动感应Activity的生命周期了。

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var observer:MyObserver

    override fun onCreate(savedInstanceState: Bundle?) 		{
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        lifecycle.addObserver(MyObserver(lifecycle))
    }


}
```

### liveData

​	LiveData是一种响应式编程组件，它可以包含任何类型的数据，并在数据发生变化时通知给观察者，可以和ViewModel结合在一起，作为持久性的数据传递媒介。

- 加入依赖

```kotlin
implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.6.2")

```

* 定义LiveData数据

​	使用单例模式 可以将 liveData 数据变成共享数据，多个Activity使用这个viewmodel时，都会共享同一份的数据实例。MutableLiveData是一种可变的LiveData，主要有三种读写数据的方法，分别是getValue(),setValue()和postValue()方法，getValue()方法用于获取LiveData中的数据，获取的数据可能为空，可以加入?:操作符设置默认值，setValue()方法用于给LiveData设置数据，只能在主线程中使用，postValue()方法用于在非主线程中设置数据。

```kotlin
class DataHosting:ViewModel() {

    companion object{
        private val _planList = MutableLiveData<ArrayList<Plan>>() // 可变部分不暴露给外部
    }

    val planList:LiveData<ArrayList<Plan>>
        get() = _planList // 暴露不可变的LiveData给外部

    public fun addPlan(plan:Plan){
        thread{
            val currentList = _planList.value ?: ArrayList()
        currentList.add(plan)
        _planList.postValue(currentList)
            
        }

    }

    public fun getPlan(index:Int):Plan{
        val currentList = _planList.value ?: ArrayList()
        return currentList[index]
    }

}
```

- 添加观察者 

​	调用LiveData的observe()方法观察数据的变化，observe()方法接收两个参数，第一个是一个LifeCycleOwner对象，就是Activity本身，传入this即可，第二个参数是一个Observe接口，当观察的数据发生变化时，回调方法就会被调用。

```kotlin
viewModel = ViewModelProvider(this).get(DataHosting::class.java)
viewModel.planList.observe(this){
	...
}
```

### Room

​	Room是Android官方推出的一个ORM框架，建立面向对象的语言和面向关系的数据库之间建立一种映射关系。

​	Room 对整个 SQLite 库进行了顶层封装,由 Entity,Dao 和 Database 这 3 部分组成

- Entity:定义封装实际数据的实体类，每个实体类都会在数据库中有一张对应的表，并且表中的列是根据实体类中的数据自动生成的
- Dao:对数据库的各项操作(CRUD)进行封装。
- Database:用于定义数据库中的关键信息，包括数据库的版本号，包括哪些实体类以及提供Dao层的访问实例

​	**使用前的配置：**

- ksp的配置

  1. 在Setting.gradle文件中加入maven

  ```kotlin
  dependencyResolutionManagement {
      repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
      repositories {
          google()
          mavenCentral()
          maven { url= uri( "https://jitpack.io") }
      }
  }
  ```

  2. 在顶层build.gradle中加入ksp

  ```kotlin
      id("com.google.devtools.ksp") version "1.9.21-1.0.15" apply false
  ```

2. 引入依赖

```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
    id("com.google.devtools.ksp")
    id("kotlin-android")
}

    implementation("androidx.room:room-runtime:2.6.1")
    ksp("androidx.room:room-compiler:2.6.1")
    implementation("androidx.room:room-ktx:2.6.1")

```

* kapt的配置：

	1. 在gradle中添加依赖

```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
        id("kotlin-kapt")
}

dependencies {

	....
    implementation("androidx.room:room-runtime:2.6.1")
    kapt("androidx.room:room-compiler:2.6.1")

}
```





 **Entity**

​	**@PrimaryKey** 注解声明主键,**autoGenerate**可以设置主键的值是否是自动的

```kotlin
@Entity
data class Plan(val title:String,val important:String,val time:String,val comment:String,val situation:String,var path:String = ""){

    @PrimaryKey(autoGenerate = false)
    var id:Long = 0

}

```



 **Dao** 

​	数据库操作对应Room的@Insert,@Delete,@Query和@Updata四种注解。其中@Insert表示会将参数中传入的对象插入到数据库中，@Delete表示会将参数传入的对象从数据库中删除， @Update表示会将参数中的对象更新到数据库中。

​	但是要实现复杂的功能就必须使用@Query，并写入SQl语句。

```kotlin
@Dao
interface PlanDao {

    @Insert
    fun insertPlan(plan:Plan):Long

    @Update
    fun updateUser(newplan:Plan)

    @Query("select * from `Plan`")
    fun loadAllPlans():List<Plan>

    @Query("delete from `Plan` where id = :id")
    fun deletePlan(id: Long)

    // 更新记录的 id 值，将满足条件的记录的 id 减 1
    @Query("UPDATE `Plan` SET id = id - 1 WHERE id > :threshold")
    fun indexAdjust(threshold: Long)

}
```



**Database**

​	需要定义好3个部分的内容：数据库的版本号，包括哪些实体类，以及能提供Dao层的访问实例

​	Database类继承于RoomDatabase类，使用abstract关键字将其声明为抽象类，提供相应的抽象方法，用于获取之前编写的Dao实例，头部使用@Database注解，声明数据库的版本号和包含的实体类，多个实体类之间用逗号隔开。

​	使用companion object定义一个单例模式，全局只存在一份Database实例，使用instance缓存Database的实例，在getDatabase()方法中判断，如果instance不为空就直接返回，否则就调用Room.databaseBuilder()方法来构建一个Database实例。

​	Room.databaseBuilder()方法接收3个参数，第一个参数为context.applicationContext，第2个参数为Database的Class类型，第三个是数据库名。

```kotlin
@Database(version = 1, entities = [Plan::class])
abstract class PlanDatabase:RoomDatabase() {

    abstract fun planDao():PlanDao

    companion object{
        private var instance:PlanDatabase ?= null

        @Synchronized
        fun getDatabase(context: Context):PlanDatabase{
            instance?.let {
                return it
            }
            return  Room.databaseBuilder(context.applicationContext,PlanDatabase::class.java,"plan_database").build().apply {
                instance = this
            }
        }
    }
}
```

​	调用 Room

​	数据库操作属于耗时操作，需要放在子线程中运行

```kotlin
planDao = PlanDatabase.getDatabase(this).planDao() // 获取DaO实例
thread{
    planDao.insertPlan(plan)
	...	
}

```

### WorkManager

- WorkManager 用于执行周期性的任务

- 引入依赖

```kotlin
implementation("androidx.work:work-runtime:2.8.1")

```

- 定义后台任务

```kotlin
class WarningWorker(val context: Context,params:WorkerParameters,val viewModel: DataHosting):Worker(context,params) {

    override fun doWork(): Result {
            //
    }
}
```

- 启动任务

```kotlin
val request = PeriodicWorkRequest.Builder(WarningWorker::class.java,1,TimeUnit.DAYS)
    .setInitialDelay(5,TimeUnit.MINUTES)
    .build()
WorkManager.getInstance(this).enqueue(request)
```

- 可以使用 addTag 增加标签,然后通过标签取消后台任务

```kotlin
WorkManager.getInstance(this).cancelAllWorkByTag()
```

- 也可以一次性取消所有后台任务请求

```kotlin
WorkManager.getInstance(this).cancelAllWork()
```



## 权限

​	Android6.0系统中加入运行时权限功能，用户不需要再安装软件的时候一次性授权所有申请的权限，而是再软件的使用过程中再对某一项权限进行授权。

​	Android将常用的权限大致分成两类，一类是普通权限，一类是危险权限。普通权限指的是那些不会直接威胁到用户的安全和隐私的权限，对于这部分权限申请，系统会自动授权，不需要用户手动操作。危险权限则表示可能会触及用户隐私或者对设备安全性造成影响的权限，对于这部分权限申请，必须由用户手动授权才可以，否则程序无法使用相应的功能。

* 普通权限

​	直接在AndroidManifest.xml文件中加入权限声明

```kotlin
<uses-permisssion android:name = "android.permission.RECEIVE_BOOT_COMPLETED"
```

* 危险权限

​	先进行权限声明

```xml
<uses-permission android:name = "android.permission.CALL_PHONE"
```

​	然后要进行运行时权限的完整流程

```kotlin
class MainActivity:AppCpmpatActivity(){
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if(!::binding.isInitialized){
            binding = ActivityMainBinding.inflate(layoutInflater)
        }
        setContentView(binding.root)

        binding.makeCall.setOnClickListener {
            if (ContextCompat.checkSelfPermission(this,android.Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED){
                ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.CALL_PHONE),1)
            }
            else{
                call()
            }
        }
    }
    
    override fun onRequestPermissionsResult(
            requestCode: Int,
            permissions: Array<out String>,
            grantResults: IntArray
        ) {
            super.onRequestPermissionsResult(requestCode, permissions, grantResults)
            when(requestCode){
                1 -> {
                    if (grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED){
                        call()
                    }
                    else{
                        Toast.makeText(this,"You denied the permission",Toast.LENGTH_SHORT).show()
                    }
                }
            }
        }
    
    private fun call(){
        val intent = Intent(Intent.ACTION_CALL)
        intent.data = Uri.parse("tel:10086")
        startActivity(intent)
    }

}
```

​	**ContextCompat.checkSelfPermission()**用于判断用户是否已经给过授权了,此方法接收两个参数，第一个参数是Context，第二个参数是具体的权限名，然后使用方法的返回值和PackageManager.PERMISSION_GRANTED作比较，相等就说明用户已经授权，不等就表示用户没有授权。如果已经授权则调用相关的逻辑操作，没有授权则需要调用**ActivityCompat.requestPermissions()**方法向用户申请授权，requestPermissions()方法接收3个参数，第一个参数要求是Activity的实例，第二个参数是一个String数组，将要申请的权限名放在数组中即可，第三个参数是请求码，需要唯一值。

​	调用完requestPermissions()方法后，系统会弹出一个权限申请的对话框，用户可以选择同一或拒绝我们的权限申请。最终会回调到**onRequestPermissionResult()**方法中，而授权的结果则会封装在grantResults参数当中，之后判断grantResults执行对应操作即可。



## 共享数据

​	ContentProvider的用法有两种：一种是使用现有的ContentProvider读取和操作相应程序中的数据，另一种是创建自己的ContentProvider，给程序的数据提供外部访问接口。如果一个应用程序通过ContentProvider对其数据提供了外部访问接口，那么任何其他的应用程序都可以对这部分数据进行访问。

### ContentProvider的基本用法

​	想要访问ContentProvider中共享的数据，需要借助ContentResolver类，可以通过Context中的getContentResolver()方法获取该类的实例。ContentResolver中提供了一系列的方法用于对数据进行增删查改操作。

​	ContentResolver中的增删查改方法不接受表名参数，而是使用一个Uri参数代替，这个参数被称为内容URI。内容URI给ContentProvider中的数据建立了唯一标识符，它主要由两部分组成：authority和path。authority是用于对不同的应用程序做区分的，一般为了避免冲突，会采用应用名的方式命名。path则是用于对同一应用程序中不同的表做区分的，通常会添加到authority后面。内容URI标准还需要再字符串的头部加上协议声明

```kotlin
content://com.example.app.provider/table1     // 表示表中所有数据
content://com.example.app.provider/table1/1   // 表中拥有相应id的数据
val uri = Uri.prase("content://com.example.app.provider/table1") // 解析成uri对象
```

​	通配符匹配

```kotlin
// 匹配任意表的内容URI
content://com.example.app.provider/*
// 匹配table1表中任意一行数据的内容URI
content://com.example.app.provider/table1/#

val table1Dir = 0
val table1Item = 1
val table2Dir = 2
val table2Item = 3

val uriMatcher = UriMatcher(UriMatcher.NO_MATCH)

uriMatcher.addURI("com.example.app.provider","table1",table1Dir)
uriMatcher.addURI("com.example.app.provider","table1/#",table1Item)
uriMatcher.addURI("com.example.app.provider","table2",table2Dir)
uriMatcher.addURI("com.example.app.provider","table2/#",table2Item)

when(uriMatcher.match(uri)){
    table1Dir-> {
        
    }
    table1Item->{
        
    }
    table2Dir->{
        
    }
    table2Item->{
        
    }
}

```

​	UriMatcher实例调用addURI方法将期望匹配的内容URI格式传递进去，UriMather的match方法对传入的Uri对象进行匹配，如果发现UriMatcher中某个内容URI格式成功匹配了该Uri对象，则会返回相应的自定义代码，然后就可以判断出调用方期望访问的到底是声明数据了。





### CRUD 操作 

#### 添加数据

```kotlin
val values = contentValuesOf("column1" to "text","column2" to 1)
contentResolver.insert(uri,values)
```

#### 删除数据

```kotlin
contentResolver.delete(uri,"column2 = ?",arrayOf("1"))
```

#### 查询数据

|  query()参数  |      对应 SQL 部分       |              描述               |
| :-----------: | :----------------------: | :-----------------------------: |
|      uri      |     from table_name      |  指定某个应用程序下的某一张表   |
|  projection   |  select column1,column2  |         指定查询的列名          |
|   selection   |   where column = value   |       指定 where 约束条件       |
| selectionArgs |                          | 为 where 中的占位符提供具体的值 |
|   sortOrder   | order by column1,column2 |     指定查询结果的排序条件      |

```kotlin
contentResolver.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI,null,null,null,null)?.apply {
    do{
            val name = getString(getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME))
            val num = getString(getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.NUMBER))

    } while(moveToNext())
    close()
}
```

#### 更新数据

```kotlin
val values = contentValuesOf("column1" to "")
contentResolver.insert(uri,values,"column1 = ? and column2 = ?",arrayOf("text","1"))
```

### 自定义的 ContentProvider 类

​	在AndroidManifest.xml中注册,可以在Android studio中直接创建该类，自动完成注册,当完成自定义的ContentProvider类时，其他的程序就可以根据内容URI来对这个程序中数据库中的表进行增删查改了。

```xml
<provider
	android:name = ".DatabaseProvider"
	android:authorities = "com.example.databaset.provider"
	android:enabled="true"
	android:exported="true">
</provider>	
	
```

​	需要重写 6 个方法，并在方法中完成具体的数据库操作。

```kotlin
class MyProvider:ContentProvider(){
    override fun onCreate():Boolean{ // 初始化ContentProvider的时候调用，通常会再这里完成对数据库的创建和升级等操作，返回true表示ContentProvider初始化成功，返回false则表示失败
        return false
    }

    override fun query(uri:Uri,projection:Array<String>?,saelection:String?,selectionArgs:Array<String>?,sortOrder:String?):Cursor?{// 从ContentProvider中查询数据，uri参数用于确定查询哪张表，projection参数用于确定查询哪些列，selection和selectionArgs参数用于约束查询哪些行，sortOredr参数用于对结果进行排序，查询的结果存放再Cursor对象中返回
        return null
    }

    override fun insert(uri:Uri,values:ContentValues?):Url??{ // 向CintentProvider中添加一条数据，uri参数用于确定要添加到的表，待添加的数据保存再values参数中，添加完成后，返回一个用于表示这条新记录的URI
        return null
    }

    override fun update(uri:Uri,selection:String?,selectionArgs:Array<String>?):Int{ // 更新ContentProvider中已有的数据，uri参数用于确定更新到哪一张表中的数据，新数据保存再values参数中，selection和selectionArgs参数用于约束更新到哪些行，受影响的函数将作为返回值返回
        return 0
    }

    override fun delete(uri:Uri,selection:String?,selectionArgs:Array<Strng>?):Int{ // 从ContentProvider中删除数据，uri参数用于确定删除哪一张表中的数据，selection和selectiojnArgs参数用与约束删除哪些行，被删除的行数将作为返回值返回。
        return 0
    }
    
    override fun getType(uri:Uri):String?{ // 根据传入的内容URI返回相应的MIME类型
        return null
    }
}
```

## 多媒体

### 通知

1. 创建通知渠道

- NotificationChannel 接收三个参数:渠道 ID,渠道名称和重要级
- 重要级有:**_IMPORTANCEE_HIGH_**,**_IMPORTANCEE_DEFAULT_**,**_IMPORTANCEE_LOW_**,**_IMPORTANCEE_MIN_**

```kotlin
// 获得noticemanager
val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
// 新建channel
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O){
    val channel = NotificationChannel("normal","Normal",NotificationManager.IMPORTANCE_DEFAULT)
    manager.createNotificationChannel(channel)
}
```

2. 创建和发送通知

- 通过**_NotificationCompat.Builder_**创建通知
- 通过**_manager.notify_**发送通知,

```kotlin
val notification = NotificationCompat.Builder(this,"normal").setContentTitle("This is content title").setContentText("This is content text").setSmallIcon(R.drawable.small_icon).setLargeIcon(BitmapFactory.decodeResource(resources,R.drawable.large_icon)).setAutoCancel(true).build()
manager.notify(1,notification)
```

3. 设置点击效果

- **_PendingIntent_**可选择**_getActivity_**方法,**_getBroadcast_**方法,**_getService_**方法
- 接收四个参数:第一个为 context,第二个为 0,第三个为 intent 对象,第四个确定行为,**_FLAG_ONE_SHOT_**,**_FLAG_NO_CREATE_**,**_FLAG_CANCEL_CURRENT_**和**_FLAG_UPDATE_CURRENT_**

```kotlin
val intent = Intent(this,NotificationActivity::class.java)
val pi = PendingIntent.getActivity(this,0,intent,PendingIntent.FLAG_UPDATE_CURRENT)
```

- 然后在 builder 里面添加 setContentIntent(pi)

```kotlin
val notification = NotificationCompat.Builder(this,"normal").setContentTitle("This is content title").setContentText("This is content text").setSmallIcon(R.drawable.small_icon).setLargeIcon(BitmapFactory.decodeResource(resources,R.drawable.large_icon)).setContentIntent(pi).setAutoCancel(true).build()
```

### 打开摄像头

* 申请camera权限

```xml
<uses-permission android:name="android.permission.CAMERA" />
```

- 注册器里面使用 BitmapFactory.decodeStream()方法将 output_image.jpg 解析成 bitmap 对象
- intent 使用**_android.media.action.IMAGE_CAPTURE_**,并 putExtra()填入**_MediaStore.EXTRA_OUTPUT,imageUri_**来指定保存路径
- 在安卓 7.0 之前,Uri 路径可以直接使用本地真实路径,而在 7.0 之后则需要用**_FileProvider.getUriForFile_**的方法来获取 Uri 路径,该方法接收三个参数,第一个为 Context 对象,第二个为字符串,第三个为 File 对象

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    private lateinit var outputImage:File
    private lateinit var imageUri:Uri
    private val requestDataLauncher =
        registerForActivityResult(ActivityResultContracts.StartActivityForResult()){
                result ->
            if (result.resultCode == RESULT_OK) {
                    val bitmap = BitmapFactory.decodeStream(contentResolver.openInputStream(imageUri))
                    binding.imageView.setImageBitmap(rotateIfRequired(bitmap))
                }
            }


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if(!::binding.isInitialized){
            binding = ActivityMainBinding.inflate(layoutInflater)
        }
        setContentView(binding.root)
        binding.takePhotoBtn.setOnClickListener {
            outputImage = File(externalCacheDir,"output_image.jpg")
            if(outputImage.exists()){
                outputImage.delete()
            }
            outputImage.createNewFile()
            imageUri = if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.N){
                FileProvider.getUriForFile(this,"com.example.cameraalbumtest.fileprovider",outputImage)
            }
            else{
                Uri.fromFile(outputImage)
            }

            //启动相机程序
            val intent = Intent("android.media.action.IMAGE_CAPTURE")
            intent.putExtra(MediaStore.EXTRA_OUTPUT,imageUri)
            requestDataLauncher.launch(intent)
        }
    }

    private fun rotateIfRequired(bitmap: Bitmap):Bitmap{
        val exif = ExifInterface(outputImage.path)
        val orientation = exif.getAttributeInt(ExifInterface.TAG_ORIENTATION,ExifInterface.ORIENTATION_NORMAL)
        return when(orientation){
            ExifInterface.ORIENTATION_ROTATE_90 -> rotateBitmap(bitmap,90)
            ExifInterface.ORIENTATION_ROTATE_180 -> rotateBitmap(bitmap,180)
            ExifInterface.ORIENTATION_ROTATE_270 -> rotateBitmap(bitmap,270)
            else -> bitmap
        }
    }

    private fun rotateBitmap(bitmap:Bitmap,degree:Int):Bitmap{
        val matrix = Matrix()
        matrix.postRotate(degree.toFloat())
        val rotatedBitmap = Bitmap.createBitmap(bitmap,0,0,bitmap.width,bitmap.height,matrix,true)
       bitmap.recycle()
        return rotatedBitmap
    }

}
```

- 因为要用到 fileprovider,所以要进行注册  
  **meta-data**指定了 Uri 共享路径需要创建\*\*

```xml
<provider
            android:authorities="com.example.cameraalbumtest.fileprovider"
            android:name="androidx.core.content.FileProvider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths"
                />
        </provider>
```

- path 为共享路径

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path
        name="my_images"
        path="/"
        />
</paths>
```
#### 在preview中预览摄像机

* 引入cmaerax依赖

```kotlin
// CameraX core library
implementation ("androidx.camera:camera-core:1.2.0-alpha02")

// CameraX Camera2 extensions
implementation ("androidx.camera:camera-camera2:1.2.0-alpha02")

// CameraX Lifecycle library
implementation ("androidx.camera:camera-lifecycle:1.2.0-alpha02")

// CameraX View class
implementation ("androidx.camera:camera-view:1.2.0-alpha02")
```

* 要在preview预览相机要准备***cameraProvider***,***preview***,***cameraSelector***

```kotlin
private var preview: Preview? = null
private var imageAnalyzer: ImageAnalysis? = null
private var camera: Camera? = null
private var cameraProvider: ProcessCameraProvider? = null
```


* 现在activity中引入preview控件,***scaleType***选项设置preview充满父布局

```xml
<androidx.camera.view.PreviewView
    android:id="@+id/view_finder"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:scaleType="fillStart" />
```

* 设置***cameraProvider***

```kotlin
// 获取相机实例
val cameraProviderFuture =
    ProcessCameraProvider.getInstance(requireContext())
cameraProviderFuture.addListener(
    {
        // 设置CameraProvider
        cameraProvider = cameraProviderFuture.get()

        // 绑定相机实例
        bindCameraUseCases()
    }, ContextCompat.getMainExecutor(requireContext())
)
```

* 进行绑定
* 相机选择器用于选择打开前置还是后置摄像头,***CameraSelector.LENS_FACING_FRONT***为前置摄像头,***CameraSelector.LENS_FACING_BACK***为后置摄像头
* ***imageAnalyzer***图像分析器用于处理每一帧的图像
* 在设置绑定之前需要调用***unbindAll***方法进行解绑

```kotlin
    private fun bindCameraUseCases() {

        // 获得CameraProvider
        val cameraProvider = cameraProvider
            ?: throw IllegalStateException("相机初始化失败")

        // 创建相机选择器
        val cameraSelector =
            CameraSelector.Builder().requireLensFacing(viewModel.getCameraModel()).build()

        // 以4:3的比例构建preview
        preview = Preview.Builder().setTargetAspectRatio(AspectRatio.RATIO_4_3)
            .setTargetRotation(fragmentCameraBinding.viewFinder.display.rotation)
            .build()

        // 使用rgb888格式解析直播流
        imageAnalyzer =
            ImageAnalysis.Builder().setTargetAspectRatio(AspectRatio.RATIO_4_3)
                .setTargetRotation(fragmentCameraBinding.viewFinder.display.rotation)
                .setBackpressureStrategy(ImageAnalysis.STRATEGY_KEEP_ONLY_LATEST)
                .setOutputImageFormat(ImageAnalysis.OUTPUT_IMAGE_FORMAT_RGBA_8888)
                .build()
                .also {
                    it.setAnalyzer(backgroundExecutor) { image ->
                        detectFace(image)
                    }
                }

        // 将camera与preview解绑
        cameraProvider.unbindAll()

        try {
            // 绑定camera和preview
            camera = cameraProvider.bindToLifecycle(
                this, cameraSelector, preview, imageAnalyzer
            )

            // 将preview渲染到控件上
            preview?.setSurfaceProvider(fragmentCameraBinding.viewFinder.surfaceProvider)
        } catch (exc: Exception) {
            Log.e(TAG, "Use case binding failed", exc)
        }
    }
```

### 从相册选择图片

- 注册器中从 result.data?.data 里面获取 uri
- 注意文件选择器的隐式启动方式

```kotlin
private val fromAlbumLauncher =
        registerForActivityResult(ActivityResultContracts.StartActivityForResult()){
            if(it.resultCode == RESULT_OK){
                // 获取选择的图片的 URI
                val selectedImageUri = it.data?.data
                if(selectedImageUri != null){
                    val bitmap = getBitmapFromUri(selectedImageUri)
                    binding.imageView.setImageBitmap(bitmap)
                }
            }
        }

binding.fromAlbumBtn.setOnClickListener {
    // 打开文件选择器
    val intent = Intent(Intent.ACTION_OPEN_DOCUMENT)
    intent.addCategory(Intent.CATEGORY_OPENABLE)
    // 指定只显示图片
    intent.type = "image/*"
    fromAlbumLauncher.launch(intent)
}

private fun getBitmapFromUri(uri:Uri) = contentResolver.openFileDescriptor(uri,"r")?.use {
    BitmapFactory.decodeFileDescriptor(it.fileDescriptor)
}

```

* 另一种解析bitmap的方法,使用ImageDecoder

```kotlin
// 解析image成bitmap对象
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) {
    val source = ImageDecoder.createSource(
        requireActivity().contentResolver,
        uri
    )
    ImageDecoder.decodeBitmap(source)
} else {
    MediaStore.Images.Media.getBitmap(
        requireActivity().contentResolver,
        uri
    )
}
    .copy(Bitmap.Config.ARGB_8888, true)
```


### 播放多媒体文件

#### 播放音频

1. 在 main 下创建 assets 目录,存放 mp3

2. 初始化 mediaPlayer

```kotlin
private fun initMediaPlayer(){
    val assetManager = assets
    val fd = assetManager.openFd("music.mp3")
    mediaPlayer.setDataSource(fd.fileDescriptor,fd.startOffset,fd.length)
    mediaPlayer.prepare()
}
```

3. 各种功能实现

```kotlin
binding.play.setOnClickListener {//播放音乐
    if(!mediaPlayer.isPlaying){
        mediaPlayer.start()
    }
}

binding.pause.setOnClickListener {//暂停音乐
    if(mediaPlayer.isPlaying){
        mediaPlayer.pause()
    }
}

binding.stop.setOnClickListener {//停止音乐
    if(mediaPlayer.isPlaying){
        mediaPlayer.reset()
        initMediaPlayer()
    }
}
```

4. 回收

```kotlin
override fun onDestroy() {
    super.onDestroy()
    mediaPlayer.start()
    mediaPlayer.release()
}
```

#### 播放视频

1. 解析 uri

````kotlin
val uri = Uri.parse("android.resource://$packageName/${R.raw.video}")
binding.videoView.setVideoURI(uri)

2. 各种功能

```kotlin
binding.play.setOnClickListener {
    if (!binding.videoView.isPlaying){
        binding.videoView.start() // 开始播放
    }
}

binding.pause.setOnClickListener {
    if (!binding.videoView.isPlaying){
        binding.videoView.start() // 暂停播放
    }
}


binding.replay.setOnClickListener {
    if (!binding.videoView.isPlaying){
        binding.videoView.resume() // 重新播放
    }
}
````

* 更多的扩展选项

```kotlin
with(fragmentGalleryBinding.videoView) {
    setVideoURI(uri)
    // 静音
    setOnPreparedListener { it.setVolume(0f, 0f) }
    // 获取焦点,e用于交互
    requestFocus()
}
```


3. 回收

```kotlin
override fun onDestroy() {
    super.onDestroy()
    binding.videoView.suspend()
}
```

## 多线程编程
### 线程

* 启动线程的方式
```kotlin
thread{

}
```

* 使用***runOnUiThread***方法回到主线程

​	UI的更新是子线程不安全的，必须要在主线程中执行

```kotlin
thread{
    runOnUiThread{
		// UI更新
    }
}
```

### 异步消息处理

​	UI的更新是子线程不安全的，必须要在主线程中执行,若想在子线程中进行UI操作，就要使用异步消息处理机制。Android中的异步消息处理主要由4部分组成：Message,Handler,MessageQueue和Looper。

* Message是在线程之间传递的消息，它可以在内部携带少量的信息，用于在不同线程之间传递数据。
* Handler主要用于发送和处理消息的。发送消息一般是使用Handler的sendMessage()方法，post()方法等，而发出的消息经过一系列处理后，最终传递到Handler的handleMessage()方法中
* MessageQueue消息队列主要用于存放所有通过Handler发送的消息。这部分消息会一直存在于消息队列中，等待被处理。每个线程中只会有一个MessageQueue对象
* Looper是每个线程中的MessageQueue的管家，调用Looper的loop()方法后，就会进入一个无限循环中，然后每当发送MessageQueue中存在一条消息时，就会将它取出，并传递到Handler的handleMessage()方法中。每个线程中只会有一个Looper对象。

​	异步消息处理的整个流程:首先需要在主线程当中创建一个Handler对象，并重写handleMessage()方法。然后当子线程中需要进行UI操作时，就创建一个Message对象，并重写handleMessage()方法。然后当子线程中需要进行UI操作时，就创建一个Message对象，并通过Handler将这条消息发送出去。之后这条消息会被添加到MessageQueue的队列中等待被处理，而Looper则会一直尝试从MessageQueue中取出待处理的信息，最后分发回Handler的handleMessage()方法中。在Looper的构造函数中传入Looper.getMainLooper()，此时handleMessage()中的代码也会在主线程中运行。

```kotlin

val imageText = 1

private val handler = object :Handler(Looper.getMainLooper()){
    override fun handleMessage(msg: Message) {
        when(msg.what){
            imageText1 -> //Ui操作

        }

    }
}


thread{
    // 利用message传递一个标识符
    val msg = Message()
    msg.what = imageText
    handler.sendMessage(msg)
}

```



### HandlerHelper

* HandlerHelper作为handler管理器，可在任意的子项fragment，协程，线程，甚至是类方法中更改其他主线程activity，fragment的UI
* 先对binding注册，然后设置Handler中的msg选项，使用handlerSendMessage来发送message

```kotlin
object HandlerHelper {

    var messagebinding: FragmentMessageBinding?= null

    fun registerMessageBinding(binding: FragmentMessageBinding){
        messagebinding = binding
    }

    val messageHandler = object :Handler(Looper.getMainLooper()){
        override fun handleMessage(msg: Message) {
            super.handleMessage(msg)
            val data = msg.data.getString("data")
            val message = messagebinding?.messageView?.text
            when(msg.what){
                BluetoothHelper.TXFlag->{
                    messagebinding?.messageView?.text = "${message}\n<-${data}"
                }
                BluetoothHelper.RXFlag->{
                    messagebinding?.messageView?.text = "${message}\n->${data}"
                }
            }
        }
    }

    fun handlerSendMessage(Data:String){
        thread {
            val message = Message()
            message.what = BluetoothHelper.TXFlag
            message.data = Bundle().apply {
                putString("data",Data)
            }
            messageHandler.sendMessage(message)
        }

    }
}
```





## 协程

​	协程允许在单线程模式下模拟多线程编程的效果，代码执行时的挂起与恢复完全是由编程语言控制的，和操作系统无关。

* 引入依赖

```kotlin
implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.1.1")
implementation("org.jetbrains.kotlinx:kotlinx-android-core:1.1.1")
```

* 开启一个协程

  runBlocking创建一个协程的作用域，可以保证协程作用域类的所有代码和子协程没有全部执行完之前可以一直阻塞当前线程

```kotlin
runBlocking{
	...
	// 开启多个协程
	launch{
		...
	}
	launch{
	
	}
	
}
```

​	使用多线程的多协程并发

```kotlin
thread{
	runBlocking{
		launch{
		
		}
		
		launch{
		
		}
	}
}
```



## Service

### Service 基本用法

- 在项目中点击 New->Service 创建 Service,可自动完成注册
- onBind 方法是抽象方法,必须重写
- onCreate 在 Service 创建时调用,onStartCommand 在 Service 每次启动时调用,onDestory 在 Service 销毁时调用

```kotlin
class MyService : Service() {

    override fun onBind(intent: Intent): IBinder {
        TODO("Return the communication channel to the service.")
    }

    override fun onCreate() {
        super.onCreate()
    }


    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        return super.onStartCommand(intent, flags, startId)
    }


    override fun onDestroy() {
        super.onDestroy()
    }
}
```

- 利用 intent 启动和停止 Service

```kotlin
binding.startServiceBtn.setOnClickListener {
    val intent = Intent(this,MyService::class.java)
    startService(intent)
}

binding.stopServiceBtn.setOnClickListener {
    val intent = Intent(this,MyService::class.java)
    stopService(intent)
}
```

### Service 与 Activity 进行通信

1. 在自定义的 Service 类中定义 Bindear 类,并通过 onBind 方法返回

```kotlin
private val mBinder = DownloadBinder()

    inner class DownloadBinder : Binder(){
        fun startDownload(){
            Log.d("MyService","startDownload executed")
        }

        fun getProgress():Int{
            Log.d("<yService","getProgress executed")
            return 0
        }
    }

    override fun onBind(intent: Intent): IBinder {
        return mBinder
    }
```

2. 在 activity 中定义 ServiceConnection 类

- **_onServiceConnected_**方法在 Activity 和 Service 成功绑定时调用

```kotlin
private lateinit var downloadBinder:MyService.DownloadBinder
    private val connection = object : ServiceConnection{
        override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
            downloadBinder = service as MyService.DownloadBinder
            downloadBinder.startDownload()
            downloadBinder.getProgress()
        }

        override fun onServiceDisconnected(p0: ComponentName?) {

        }

    }
```

3. 通过 intent 进行绑定和取消绑定

- bindService 接收三个参数:第一个参数为 intent 对象,第二个为 ServiceConnection 实例,第三个为标志位,**_BIND_AUTO_CREATE_**表示在 Activity 和 Service 绑定后会自动启动 Service

```kotlin
binding.bindServiceBtn.setOnClickListener {
    val intent = Intent(this,MyService::class.java)
    bindService(intent,connection,Context.BIND_AUTO_CREATE) // 绑定Service
}

binding.unBindServiceBtn.setOnClickListener {
    unbindService(connection)
}
```

### 前台 Service

- 只需修改**_OnCreate()_**方法即可
- 注意调用了**_startForeground_**方法,接收两个参数,第一个为 id,第二个为 Notification 对象

```kotlin
override fun onCreate() {
    super.onCreate()
    // 获得noticemanager
    val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
    // 新建channel
    if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O){
        val channel = NotificationChannel("my_service","前台Service通知",NotificationManager.IMPORTANCE_DEFAULT)
        manager.createNotificationChannel(channel)
    }
    val intent = Intent(this,MainActivity::class.java)
    val pi = PendingIntent.getActivity(this,0,intent,PendingIntent.FLAG_UPDATE_CURRENT)
    val notification = NotificationCompat.Builder(this,"normal").setContentTitle("This is content title").setContentText("This is content text").setSmallIcon(R.drawable.small_icon).setLargeIcon(
        BitmapFactory.decodeResource(resources,R.drawable.large_icon)).setContentIntent(pi).setAutoCancel(true).build()
    startForeground(1,notification)
}
```

- 需要进行权限声明

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>

```

### IntentService

- 将 Service 中的耗时任务装入到子线程中运行

```kotlin
class MyIntentService:IntentService("MyIntentService"){
    override fun onHandleIntent(intent:Intent){
        // 耗时任务
    }

    override fun onDestory(){
        super().onDestory()
    }
}
```

## 网络编程

### WebView

- 在 xml 文件添加 wrbview

```xml
<WebVIew
    android:id = "@+id/webVoew"
    anrdoid:layout_width = "match_parent"
    android:layout_height = "match_parent"
    />
```

- 想关设置
- **_settings_**方法可以设置浏览器属性

```kotlin
binding.webView.settings.javaScriptEnabled = true
binding.webView.webViewClient = WebViewClient()
binding.webView.loadUrl("heeps:./.www.baidu.com")
```

- 需要加入权限声明

```xml
<uses-permission android:name="android.permission.INTERNET"/>

```

### Retrofit 构建器

- 需要在 gradle 中添加闭包

```kotlin
implementation("com.squareup.retrofit2:retrofit:2.6.1")
implementation("com.squareup.retrofit2:converter-gson:2.6.1")
```

- Retrofit 借助 GSON 将 JSON 数据转换成对象

```kotlin
data class App(val id:STring,val name:String,val version:String)
```

- 设置接口

```kotlin
interface AppService{
    @GET("get_data.json") //子目录
    fun getAppData():Call<List<APP>>
}
```

- 封装代理对象
- **_addConverterFactory_**指定解析 JSON 的转换库

```kotlin
object SAerviceCreate{

    private const val BASE_URL = "根目录"

    private val retrofir = Retrofir.Builder().baseUrl(BASE_URL).addConverterFactory(GsonConvertFactory.create()).build()

    inline fun <reified T> create():T = create(T::class.java)
}
```

- 启动接口

```kotlin
val appService = ServiceCreator.create<AppService>()
```

- 进行解析
- 发送请求的时候,Retrofit 自动在内部开启子线程,当数据回调到 Callback 中之后,Retrofit 自动切换回主线程,Callback 的**_onResponse_**方法中,调用**_body_**方法获得解析后的对象

```kotlin
appService.getAppData().qnqueue(object:Callback<List<App>>{
    override fun onResponsea(call:Call<List<App>>,response:Response<List<App>>){
        val list = response.body()
        // 查询
    }
    override fun onFailure(call:Call<List<App>,t:Throwable>){
        t.printStackTrace()
    }

})
```

- 记得权限声明

```xml
<uses-permission android:name="android.permission.INTERNET"/>

```

## Material Design

* 引入依赖
```kotlin
implementation ("com.google.android.material:material:1.1.0")
```

### ToolBar

1.  在 res/values/themes 中修改主题**_Theme.AppCompat.Light.NoActionBar_**

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.MaterialDesign" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/design_default_color_primary</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/white</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorOnSecondary">@color/black</item>

        <!-- Status bar color. -->
        <item name="android:statusBarColor">?attr/colorPrimaryVariant</item>
        <!-- Customize your theme here. -->
    </style>
</resources>
```

2.  修改 activity_main.xml 中的代码

- 注意加入 xmlns:app="http://schemas.android.com/apk/res-auto"命名空间,兼容旧系统

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/actionBarSize"
        android:background="@color/design_default_color_primary"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
        />


</FrameLayout>
```

3. 在 MainActivity 中修改代码

- 在 onCreate 中加入**_setSupportActionBar_**方法载入 Toolbar

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    if (!::binding.isInitialized){
        binding = ActivityMainBinding.inflate(layoutInflater)
    }
    setContentView(binding.root)
    setSupportActionBar(binding.toolbar)
}
```

4. 创建 menu,自定义功能

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/backup"
        android:icon="@drawable/ic_backup"
        android:title="Backup"
        app:showAsAction="always"

        />

    <item
        android:id="@+id/delete"
        android:icon="@drawable/ic_delete"
        android:title="Delete"
        app:showAsAction="ifRoom"
        />


    <item
        android:id="@+id/settings"
        android:icon="@drawable/ic_settings"
        android:title="Settings"
        app:showAsAction="never"
        />


</menu>
```

- **_menuInflater.inflate_**方法载入 menu,**_onOptionsItemSelected_**设置按钮点击效果

```kotlin
override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    menuInflater.inflate(R.menu.toolbar,menu)
    return true

}

override fun onOptionsItemSelected(item: MenuItem): Boolean {
    when(item.itemId){
        R.id.backup -> Toast.makeText(this,"You clicked Backup",Toast.LENGTH_SHORT).show()
        R.id.delete -> Toast.makeText(this,"You clicked Delete",Toast.LENGTH_SHORT).show()
        R.id.settings -> Toast.makeText(this,"You clicked Settings",Toast.LENGTH_SHORT).show()
    }
    return true
}
```

### 滑动菜单

#### DrawerLayout

- DrawerLayout 的布局有两个,一个是主屏幕中显示的内容,一个是滑动窗口显示的内容
- 这里的滑动窗口使用了**_NavigationView_**来指定 header 和 body 内容
- 第二个布局的**_android:layout_gravity_**必须要写,这里使用了**_start_**来根据系统语言进行滑动菜单滑动方向的判断

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"

    android:layout_height="match_parent"
    >

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?android:attr/actionBarSize"
            android:background="@color/design_default_color_primary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
            />

    </FrameLayout>

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:menu="@menu/nav_menu"
        app:headerLayout="@layout/nav_header"
        />



</androidx.drawerlayout.widget.DrawerLayout>
```

- 设置滑动菜单的导航按钮
- **_setSupportActionBar_**方法用于获得 ActionBar 实例,使用**_setDisplayHomeAsUpEnabled_**显示导航按钮,**_setHomeAsUpIndicator_**设置导航按钮图标,最后在**_onOptionsItemSelected_**中,设置**_android.R.id.home -> binding.drawerLayout.openDrawe_**,传入 xml 一致的参数

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if (!::binding.isInitialized){
            binding = ActivityMainBinding.inflate(layoutInflater)
        }
        setContentView(binding.root)
        setSupportActionBar(binding.toolbar)
        supportActionBar?.let{
            it.setDisplayHomeAsUpEnabled(true)
            it.setHomeAsUpIndicator(R.drawable.ic_menu)
        }

    }
}

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when(item.itemId){
            R.id.backup -> Toast.makeText(this,"You clicked
            Backup",Toast.LENGTH_SHORT).show()
            R.id.delete -> Toast.makeText(this,"You clicked Delete",Toast.LENGTH_SHORT).show()
            R.id.settings -> Toast.makeText(this,"You clicked Settings",Toast.LENGTH_SHORT).show()
            android.R.id.home -> binding.drawerLayout.openDrawer(GravityCompat.START)
        }
        return true
    }
```

#### NavigationView

- 可以设定滑动菜单的头部和主页部分,使用该控件需要引入相应的闭包

```kotlin
implementation("com.google.android.material:material:1.9.0")
implementation("de.hdodenhof:circleimageview:3.0.1")
```

- 准备好 menu 和 headerLayout

- menu 作为显示具体的菜单项
- group 的 checkableBehavior 属性指定为 single 表示组中的所有菜单项只能单选

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <group android:checkableBehavior="single">
        <item
            android:id="@+id/navCall"
            android:icon="@drawable/nav_call"
            android:title="Call"
            />

        <item
            android:id="@+id/navFriends"
            android:icon="@drawable/nav_friends"
            android:title="Friends"
            />

        <item
            android:id="@+id/navLocation"
            android:icon="@drawable/nav_location"
            android:title="Location"
            />

        <item
            android:id="@+id/navMail"
            android:icon="@drawable/nav_mail"
            android:title="Mail"
            />


        <item
            android:id="@+id/navTask"
            android:icon="@drawable/nav_task"

            android:title="Tasks"
            />


    </group>

</menu>
```

- headerLayout 作为头部布局
- **_circleimageview_**可将图片圆化

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="180dp"
    android:padding="10dp"
    android:background="@color/design_default_color_primary">

    <de.hdodenhof.circleimageview.CircleImageView
        android:id="@+id/iconImage"
        android:layout_width="70dp"
        android:layout_height="70dp"
        android:src="@drawable/nav_icon"
        android:layout_centerInParent="true"/>

    <TextView
        android:id="@+id/mailText"

        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:text="tonygreendev@gmail.com"
        android:textColor="#FFF"
        android:textSize="14sp"
        />

    <TextView
        android:id="@+id/userText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/mailText"
        android:text="Tony Green"
        android:textColor="#FFF"
        android:textSize="14sp"
        />

</RelativeLayout>
```

- 设置菜单项的点中事件
- **_setCheckedItem_**为设置默认选中

```kotlin
binding.navView.setCheckedItem(R.id.navCall)
binding.navView.setNavigationItemSelectedListener {
    binding.drawerLayout.closeDrawers()
    true
}
```

### 悬浮按钮

- **_app:elevation_**可设置浮动高度

```xml
<com.google.android.material.floatingactionbutton.FloatingActionButton
    android:id="@+id/fab"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="bottom|end"
    android:layout_margin="16dp"
    android:contentDescription="Enter How Much Cookies You Want"
    android:src="@drawable/ic_done"
    app:elevation="8dp"
    />
```

- 设置 Snackbar 点击事件
- 调用 Snackbar 的**_make_**方法创建 Snackbar 对象,make 方法需要传入一个 VIew,Snackbar 会自动查找最外层的布局,用于展示提示信息,第二个参数是 SNackbar 中显示的内容,第三个参数是 Snackbar 显示的时长
- 然后调用**_setAction_**方法设置一个动作,点击事件后弹出 Toast 提示

```kotlin
binding.fab.setOnClickListener{view->
    Snackbar.make(view,"Data deleted",Snackbar.LENGTH_SHORT).setAction("Undo"){
        Toast.makeText(this,"Data restored",Toast.LENGTH_SHORT).show()
    }.show()
}
```

- 将**_FrameLayout_**换成**_CoordinatorLayout_**,自动调整**_FloatingActionButton_**的位置

* **_CoordinatorLayout_**可以监听器所有子控件的各种事件,并自动帮助我们做出最合理的响应

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/actionBarSize"
        android:background="@color/design_default_color_primary"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
        />

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="16dp"
        android:contentDescription="Enter How Much Cookies You Want"
        android:src="@drawable/ic_done"
        app:elevation="8dp"
        />

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

### 卡片式布局

​	常作为recycleView的ListItem的视图布局

- **_app:elevation_**属性指定卡片的高度,高度值越大,投影范围越大,投影效果越淡,高度值越小,投影范围越小,投影效果越浓
- **_app:cardCornerRadius_**属性指定卡片圆角的弧度,数值越大,圆角弧度越大

```xml
<com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="5dp"
    app:elevation="5dp"
    app:cardCornerRadius="4dp"
    >

    <TextView
        android:id="@+id/fruitName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:layout_margin="5dp"
        android:textSize="16sp"
        />

</com.google.android.material.card.MaterialCardView>
```

#### 下拉刷新

- 添加依赖

```kotlin
implementation("androidx.swiperefreshlayout:swiperefreshlayout:1.1.0")

```

- recyclerView 的**\*app:layout_behavior**布局也要写到**_SwipeRefreshLayout_**内

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        ...

        <androidx.swiperefreshlayout.widget.SwipeRefreshLayout
            android:id="@+id/swipeRefresh"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior"
            >

            <androidx.recyclerview.widget.RecyclerView
                android:id="@+id/recyclerView"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

        </androidx.swiperefreshlayout.widget.SwipeRefreshLayout>


        ...

    </androidx.coordinatorlayout.widget.CoordinatorLayout>

    ...


</androidx.drawerlayout.widget.DrawerLayout>
```

- **_setColorSchemeResources_**方法设置刷新条颜色
- **_setOnRefreshListener_**设置监听事件
- **_refreshFruits_**方法中先在子线程中睡眠 2 秒,再返回主线程,避免刷新过快,最后调用**_setRefreshing_**方法表示刷新事件结束,隐藏刷新条

```kotlin

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    ...

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if (!::binding.isInitialized){
            binding = ActivityMainBinding.inflate(layoutInflater)
        }
        setContentView(binding.root)


        ...


        binding.swipeRefresh.setColorSchemeResources(com.google.android.material.R.color.design_default_color_primary)

        binding.swipeRefresh.setOnRefreshListener {
            refreshFruits(adapter)
        }


    }



    ...

    private fun refreshFruits(adapter:FruitAdapter){
        thread {
            Thread.sleep(2000)
            runOnUiThread{
                initFruits()
                adapter.notifyDataSetChanged()
                binding.swipeRefresh.isRefreshing = false
            }
        }
    }

}
```

#### 折叠式标题栏

- **_CollapsingToolbarLayout_**只能作为 AppBarLayout 的直接子布局来使用,而**_AppBarLayout_**又必须是**_CoordinatorLayout_**的子布局
- **_CollapsingToolbarLayout_**的**_contentScrim_**属性指定折叠背景色,**_app:layout_scrollFlags_**属性的 scroll 表示会随着内容详情的滚动一起滚动,exitUntilCollapsed 代表随着滚动完成折叠之后就保留再界面上,不再移出屏幕
- **_ToolBar_**的**_layout_collapseMode_**属性指定为**_pin_**,表示再折叠的过程中位置保持不变,**_ImageView_**指定为**_parallax_**表示再折叠的过程中产生一定的错位偏移
- **_NestedScrollView_**在**_ScrollView_**的基础上增加了嵌套响应滚动的功能
- **_FloatingActionButton_**的**_app:layout_anchor_**属性指定锚点
- 对 ImageView 和其所有的子布局都设置**_android:fitsSystemWindows_**属性让背景图和系统状态栏融合,还需要创建主题,然后在**_AndroidManifest_**中使用这个主题,

```xml
    <style name="FruitActivityTheme" parent="Theme.MaterialDesign">
        <item name="android:statusBarColor">@android:color/transparent</item>
    </style>

```

```xml
<activity
    android:name=".FruitActivity"
    android:theme="@style/FruitActivityTheme"
    android:exported="false" />
<activity
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity"
    android:fitsSystemWindows="true">

    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/appBar"
        android:layout_width="match_parent"
        android:layout_height="250dp"
        android:fitsSystemWindows="true">

        <com.google.android.material.appbar.CollapsingToolbarLayout
            android:id="@+id/collapsingToolbar"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:contentScrim="@color/design_default_color_primary"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"
            android:fitsSystemWindows="true">

            <ImageView
                android:id="@+id/fruitImageVIew"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="centerCrop"
                app:layout_collapseMode="parallax"
                android:fitsSystemWindows="true"/>

            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"
                />


        </com.google.android.material.appbar.CollapsingToolbarLayout>


    </com.google.android.material.appbar.AppBarLayout>

    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <com.google.android.material.card.MaterialCardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content">

                <TextView
                    android:id="@+id/fruitContentText"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_margin="10dp"
                    />

            </com.google.android.material.card.MaterialCardView>
        </LinearLayout>

    </androidx.core.widget.NestedScrollView>

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:src="@drawable/ic_comment"
        app:layout_anchor="@id/appBar"
        app:layout_anchorGravity="bottom|end"

        />


</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

- 调用**_CollapsingToolbarLayout_**的**_setTitle_**方法设置当前界面的标题
- 在**_onOptionsItemSelected_**方法处理 home 按钮的点击事件

```kotlin
class FruitActivity : AppCompatActivity() {

    private lateinit var binding:ActivityFruitBinding

    companion object{
        const val FRUIT_NAME = "fruit_name"
        const val FRUIT_IMAGE_ID = "fruit_image_id"
    }



    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        if (!::binding.isInitialized){
            binding = ActivityFruitBinding.inflate(layoutInflater)
        }
        setContentView(binding.root)

        val fruitName = intent.getStringExtra(FRUIT_NAME) ?: ""
        val fruitImageId = intent.getIntExtra(FRUIT_IMAGE_ID,0)
        setSupportActionBar(binding.toolbar)
        supportActionBar?.setDisplayHomeAsUpEnabled(true)
        binding.collapsingToolbar.title = fruitName
        Glide.with(this).load(fruitImageId).into(binding.fruitImageVIew)
        binding.fruitContentText.text = generateFruitContent(fruitName)
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when(item.itemId){
            android.R.id.home->{
                finish()
                return true
            }
        }
        return super.onOptionsItemSelected(item)
    }

    private fun generateFruitContent(fruitName:String) = fruitName.repeat(500)
}
```

- Adapter

```kotlin
class FruitAdapter(val context: Context,val fruitList:List<Fruit>): RecyclerView.Adapter<FruitAdapter.ViewHolder>() {
    inner class ViewHolder(val binding:FruitItemBinding) : RecyclerView.ViewHolder(binding.root)

    ...

    override fun onBindViewHolder(holder:ViewHolder,position:Int){
        val fruit = fruitList[position]
        // 进行视图的赋值,监听等等操作
        holder.binding.fruitName.text = fruit.name
        Glide.with(context).load(fruit.imageId).into(holder.binding.fruitImage)
        holder.itemView.setOnClickListener{
            val position = holder.absoluteAdapterPosition
            val fruit = fruitList[position]
            val intent = Intent(context,FruitActivity::class.java).apply {
                putExtra(FruitActivity.FRUIT_NAME,fruit.name)
                putExtra(FruitActivity.FRUIT_IMAGE_ID,fruit.imageId)
            }
            context.startActivity(intent)
        }
    }

    override fun getItemCount() = fruitList.size
}
```

### BottomNavigationView
* BottomNavigationView封装了导航栏和fragment,可便捷的进行导航栏对fragment的快速切换

* 使用RelativeLayout布局

* 引入依赖

```kotlin
implementation ("androidx.navigation:navigation-fragment-ktx:2.5.3")
implementation ("androidx.navigation:navigation-ui-ktx:2.5.3")
```

1. 设置导航栏

* 新建menu目录，添加menu_bottom_nav.xml,作为导航栏,android:icon中填入对应的图标

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@id/camera_fragment"
        android:icon="@drawable/ic_baseline_photo_camera_24"
        android:title="@string/menu_camera" />

    <item
        android:id="@id/gallery_fragment"
        android:icon="@drawable/ic_baseline_photo_library_24"
        android:title="@string/menu_gallery" />
</menu>
```

* 在activity布局中引入BottomNavigationView,menu选项用于指定对应的导航栏布局

```xml
<com.google.android.material.bottomnavigation.BottomNavigationView
    android:id="@+id/navigation"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_alignParentBottom="true"
    app:itemIconTint="@color/bg_nav_item"
    app:itemTextColor="@color/bg_nav_item"
    app:menu="@menu/menu_bottom_nav" />
```

* 在***activity***引入***FragmentContainerView***用作***fragment***容器,***name***选项用于设置***NavigationView***导航宿主,***navGraph***选项用于设置***fragment***集合布局的xml导航图文件,***keepScreenOn***选项设置屏幕常亮,***defaultNavHost***选项设置默认的导航宿主

```xml
<androidx.fragment.app.FragmentContainerView
    android:id="@+id/fragment_container"
    android:name="androidx.navigation.fragment.NavHostFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_marginTop="?android:attr/actionBarSize"
    android:background="@android:color/transparent"
    android:keepScreenOn="true"
    app:defaultNavHost="true"
    app:navGraph="@navigation/nav_graph"
    tools:context=".MainActivity" />
```

2. 设置fragment

* 新建fragment

* 新建navigation文件夹,添加nav_graph导航图xml

* ***startDestination***选项用于设置导航图的起始fragment,

* ***action***定义了从一个 Fragment 到另一个 Fragment 的操作。它表示导航图中的转换，包括源 Fragment 和目标 Fragment，以及在何时发生此转换。***destinatio***指定操作的目标 Fragment，表示执行操作后应该导航到的 Fragment。***popUpTo*** 设置达到目的地后将tag之上的fragment弹出， ***popUpToInclusive***为false时不会将tag弹出。通常，这用于在导航后从返回栈中删除 Fragment，以确保返回操作的行为符合预期。

* fragment通过id于menu中的item进行关联

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/nav_graph"
    app:startDestination="@id/permissions_fragment">

    <fragment
        android:id="@+id/permissions_fragment"
        android:name="com.google.mediapipe.examples.facelandmarker.fragment.PermissionsFragment"
        android:label="PermissionsFragment">

        <action
            android:id="@+id/action_permissions_to_camera"
            app:destination="@id/camera_fragment"
            app:popUpTo="@id/permissions_fragment"
            app:popUpToInclusive="true" />

    </fragment>

    <fragment
        android:id="@+id/camera_fragment"
        android:name="com.google.mediapipe.examples.facelandmarker.fragment.CameraFragment"
        android:label="CameraFragment">

        <action
            android:id="@+id/action_camera_to_permissions"
            app:destination="@id/permissions_fragment"
            app:popUpTo="@id/camera_fragment"
            app:popUpToInclusive="true" />
    </fragment>

    <fragment
        android:id="@+id/gallery_fragment"
        android:name="com.google.mediapipe.examples.facelandmarker.fragment.GalleryFragment"
        android:label="GalleryFragment" />
</navigation>
```

3. 最后在main中设置导航控制器

```kotlin
val navHostFragment =
    supportFragmentManager.findFragmentById(R.id.fragment_container) as NavHostFragment
val navController = navHostFragment.navController
binding.navigation.setupWithNavController(navController)
```

## 第三方库

### AndroidFilePicker

|<https://gitee.com/codream/AndroidFilePicker>

- 用于显示指定路径的文件浏览器

- 添加仓库

```kotlin
allprojects {
    repositories {
	    ...
    	maven { url 'https://jitpack.io' }
    }
}
```

- 添加依赖

```kotlin
dependencies {
    implementation 'me.rosuh:AndroidFilePicker:0.8.3'
}
```

- 简单链式调用

```kotlin
FilePickerManager
    .from(this@MakePlanActivity)
    .setCustomRootPath("${this.filesDir}/${path}")
    .forResult(FilePickerManager.REQUEST_CODE)
```

### DateTimePicker

|<https://github.com/loper7/DateTimePicker>

- DateTimePicker 是一个高颜值日期时间选择器；极简 API，内置弹窗，支持农历日期显示，简单适配深色模式，可动态配置样式及主题，选择器支持完全自定义 UI。

- 引入仓库

```kotlin
allprojects {
	repositories {
		...
		maven { url "https://jitpack.io" }
	}
}
```

- 引入依赖

```kotlin
dependencies {
    ...
    implementation 'com.google.android.material:material:1.1.0' //为了防止不必要的依赖冲突，0.0.3开始需要自行依赖google material库
    implementation 'com.github.loper7:DateTimePicker:0.6.3'//具体版本请看顶部jitpack标识，如0.6.3,仅支持androidx
}
```

- 简单使用

```kotlin
CardDatePickerDialog.builder(this)
    .setTitle("设置开始时间")
    .setDisplayType(DateTimeConfig.MONTH,DateTimeConfig.DAY,DateTimeConfig.HOUR,DateTimeConfig.MIN)
    .showBackNow(false)
    .setOnChoose {
        val selectTime = Date(it)
        val dateFormat = SimpleDateFormat("MM-dd HH:mm", Locale.getDefault())
        val formattedTime = dateFormat.format(selectTime)
        binding.startTime.text = formattedTime
    }
    .build().show()
```
