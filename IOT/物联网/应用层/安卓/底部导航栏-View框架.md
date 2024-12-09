## 介绍
导航组件由三个关键部分组成，这三个部分协同工作。它们是：
1. **导航图**（新 XML 资源）- 这是在一个集中位置包含所有导航相关信息的 XML 资源。其中包括应用内的所有位置（称为“目的地”）以及用户在应用中可采取的可能路径。
2.  **NavHostFragment**（布局 XML 视图）- 这是添加到布局中的特殊微件。它会显示导航图中的不同目的地。
3. **NavController**（Kotlin/Java 对象）- 这是用于跟踪导航图中当前位置的对象。当您在导航图中移动时，它会编排 `NavHostFragment` 中目的地内容的交换。

实现底部导航栏用到`BottomNavigationView`+`navigation`的技术。

以两个fragment作为切换的页面
`fragment_home`
```xml
<?xml version="1.0" encoding="utf-8"?>  
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    tools:context=".fragment.fragment_home">  
  
    <!-- TODO: Update blank fragment layout -->  
    <TextView  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:text="主页面" />  
  
</FrameLayout>
```

`fragment_dashboard`
```xml
<?xml version="1.0" encoding="utf-8"?>  
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    tools:context=".fragment.fragment_dashboard">  
  
    <!-- TODO: Update blank fragment layout -->  
    <TextView  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:text="我的信息" />  
  
</FrameLayout>
```

## 导入依赖
```xml
dependencies {   
    // Views/Fragments Integration  
    implementation ("androidx.navigation:navigation-fragment:2.8.4")  
    implementation ("androidx.navigation:navigation-ui:2.8.4")  
}
```

##  BottomNavigationView的实现
1. 在`res`新建菜单目录`menu`，创建菜单布局`bottom_nav_menu.xml`
![image.png|500](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/07/22-25-46-70277086e635d7fe6c36e10d9ecb8976-20241207222544-c1dd22.png)


2. 写入相关的布局信息
```xml
<?xml version="1.0" encoding="utf-8"?>  
<menu xmlns:android="http://schemas.android.com/apk/res/android">  
    <item  
        android:id="@+id/navigation_item1"  
        android:icon="@drawable/icons/icon_main"  
        android:title="主界面" />  
    <item        
	    android:id="@+id/navigation_item2"  
        android:icon="@drawable/icons/icon_mine"  
        android:title="我的信息" />  
  
</menu>
```

注意：
* 每个`item`代表一个底部导航栏的交互按键，`icon`属性为对应的图片路径，`title`为对应的文字说明


3. 在主界面的`xml`中加入`BottomNavigationView`的布局，并且关联菜单布局，由属性`menu`进行关联
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:id="@+id/main"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical"  
    tools:context=".MainActivity">  
  
  
  
  
    <com.google.android.material.bottomnavigation.BottomNavigationView        
	    android:id="@+id/nav_view"  
        android:layout_width="match_parent"  
        android:layout_height="0dp"  
        android:layout_weight="1"  
        app:menu="@menu/bottom_nav_menu" />  
  
</LinearLayout>
```



## navigation的实现
### **导航图**
1. 在`res`目录下新建导航布局文件`navigation`，新建`navigation.xml`
![image.png|329](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/https/cdn.jsdelivr.net/gh/xuezhaorong/Picgo/Source/fix-dir/picgo/picgo-clipboard-images/2024/12/07/2024/12/08/22-32-06-42a18c85dc1eb9c767150aa26b33b632-23-21-49-42a18c85dc1eb9c767150aa26b33b632-20241207232148-eb4d70-16fa7c.png)

2. 写入布局内容
```xml
<?xml version="1.0" encoding="utf-8"?>  
<navigation xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:id="@+id/navigation"  
    app:startDestination="@id/navigation_item1">  
  
    <fragment  
        android:id="@+id/navigation_item1"  
        android:name="com.example.project.fragment.fragment_home"  
        tools:layout="@layout/fragment_home" />  
  
    <fragment        
	    android:id="@+id/navigation_item2"  
        android:name="com.example.project.fragment.fragment_dashboard"  
        tools:layout="@layout/fragment_dashboard" />  
  
</navigation>
```

 注意：
 * `startDestination`属性为起始的导航目的地，即一开始显示的fragment
 * `name`为fragment类在java包的目录
 * `layout`为fragment的布局路径
 * `id`属性要对应`menu`中每个item的`id`

### NavHostFragment
在`Main Activity`的布局`activity_main.xml`中添加`NavHostFragment`
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:id="@+id/main"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical"  
    tools:context=".MainActivity">  
  
  
  
    <fragment  
        android:id="@+id/nav_host_fragment"  
        android:name="androidx.navigation.fragment.NavHostFragment"  
        android:layout_width="match_parent"  
        android:layout_height="0dp"  
        android:layout_weight="9"  
        app:defaultNavHost="true"  
        app:navGraph="@navigation/navigation" />  
  
    <com.google.android.material.bottomnavigation.BottomNavigationView        
	    android:id="@+id/nav_view"  
        android:layout_width="match_parent"  
        android:layout_height="0dp"  
        android:layout_weight="1"  
        app:menu="@menu/bottom_nav_menu" />  
  
</LinearLayout>
```

注意：
* `navGraph`设置为导航图的xml路径

### NavController
在`Main Activity`中获取`navController`
```kotlin
val navView = binding.navView  
  
val navController = findNavController(R.id.nav_host_fragment)  
  
navView.setupWithNavController(navController)
```

