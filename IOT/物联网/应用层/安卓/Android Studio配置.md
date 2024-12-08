## 安装依赖环境
1. 安装java17，官网地址：[Java Archive Downloads - Java SE 17](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/08/17-50-36-5eb52a7ef2a7618590e5a0e1bc8c4146-20241208175035-7e4a8d.png)

2. 安装gradle，官网地址：[Gradle Build Tool](https://gradle.org/)
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/08/17-52-36-29ea86e4c7d66826cdc373ab619e3c41-20241208175235-d847d5.png)


3. 安装Android Studio，官网地址：https://developer.android.google.cn/develop?hl=zh-cn
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/08/17-54-25-6fdbaa5ad81270421295e791869ae792-20241208175424-a66872.png)

## 配置gradle本地仓库
1. 在Gradle目录下新建文件夹`repository`作为本地仓库存放所有下载的内容。
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/08/17-56-03-7517a4622fd3b484ec8c8b1e91ab8ac2-20241208175602-7f3680.png)

2. 添加Gradle和Gradle仓库的环境变量。
![image.png|600](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/08/17-58-29-2d01030fb4a10b59007af61633907cd0-20241208175829-77de19.png)


3. 在Android Studio中打开项目，设置gradle为本地，设置java为本地
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/08/18-04-58-ae1d50f0a98ea03f12570537fe117913-20241208180457-4d4258.png)

## 网络配置
因为Android Studio下载的资源多数来源于国外，需要配置网络环境避免下载过于缓慢或者失败，有两种方式，使用本地代理或者换源。
### 本地代理
在设置中的`proxy`中切换到手动代理，设置本地代理地址和端口： 
![image.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/08/18-09-56-a868f923551fdb75638a4c4c371c954f-20241208180956-df011f.png)

### 换源
在项目的`settings.gradle.kts`中替换国内源
```bash
pluginManagement {  
    repositories {  
        maven { url=uri ("https://www.jitpack.io")}  
        maven { url=uri ("https://maven.aliyun.com/repository/releases")}  
        maven { url=uri ("https://maven.aliyun.com/repository/google")}  
        maven { url=uri ("https://maven.aliyun.com/repository/central")}  
        maven { url=uri ("https://maven.aliyun.com/repository/gradle-plugin")}  
        maven { url=uri ("https://maven.aliyun.com/repository/public")}  
        google()  
        mavenCentral()  
        gradlePluginPortal()  
    }  
}  
dependencyResolutionManagement {  
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)  
    repositories {  
        maven { url=uri ("https://www.jitpack.io")}  
        maven { url=uri ("https://maven.aliyun.com/repository/releases")}  
        maven { url=uri ("https://maven.aliyun.com/repository/google")}  
        maven { url=uri ("https://maven.aliyun.com/repository/central")}  
        maven { url=uri ("https://maven.aliyun.com/repository/gradle-plugin")}  
        maven { url=uri ("https://maven.aliyun.com/repository/public")}  
        google()  
        mavenCentral()  
    }  
}
```