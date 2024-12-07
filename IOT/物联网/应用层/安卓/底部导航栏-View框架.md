
## 导入依赖
```xml
dependencies {   
    // Views/Fragments Integration  
    implementation ("androidx.navigation:navigation-fragment:2.8.4")  
    implementation ("androidx.navigation:navigation-ui:2.8.4")  
}
```


## 设计菜单布局
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

每个`item`代表一个底部导航栏的交互按键，`icon`属性为对应的图片路径，`title`为对应的文字说明

3. 在主界面的`xml`中加入`BottomNavigationView`的布局，并且关联菜单布局，由属性`menu`进行关联
```xml
<com.google.android.material.bottomnavigation.BottomNavigationView  
    android:id="@+id/nav_view"  
    android:layout_width="match_parent"  
    android:layout_height="wrap_content"  
    app:layout_constraintBottom_toBottomOf="parent"  
    app:layout_constraintStart_toStartOf="parent"  
    android:layout_marginStart="0dp"  
    android:layout_marginEnd="0dp"  
    app:menu="@menu/bottom_nav_menu" />
```

## 设置导航
1. 在`res`目录下新建导航布局文件`navigation`，新建`navigation.xml`
![image.png|329](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/07/23-21-49-42a18c85dc1eb9c767150aa26b33b632-20241207232148-eb4d70.png)

2. 写入布局内容
```xml
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/mobile_navigation"
    app:startDestination="@+id/navigation_home">

    <fragment
        android:id="@+id/navigation_item1"
        android:name="per.wsj.bottommenu.ui.fragment.Fragment1"
        android:label="@string/title_home"
        tools:layout="@layout/fragment_home" />

    <fragment
        android:id="@+id/navigation_item2"
        android:name="per.wsj.bottommenu.ui.fragment.Fragment2"
        android:label="@string/title_dashboard"
        tools:layout="@layout/fragment_dashboard" />

</navigation>

```

属性`id`必须与菜单布局`xml`中的`item`的`id`一一对应，属性`layout`绑定布局中的`fragment`，属性`startDestination`为一开始显示的fragment



3. 在主界面的`xml`中加入`fragment`的布局作为导航的的开始
```xml
 <fragment
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:layout_constraintBottom_toTopOf="@id/nav_view"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:navGraph="@navigation/mobile_navigation" />
```

属性`navGrpah`设置与`navigation`相同。


4. 最后设置导航跳转
```kotlin
val navView: BottomNavigationView = findViewById(R.id.nav_view)
val navController = findNavController(R.id.nav_host_fragment)
navView.setupWithNavController(navController)
```