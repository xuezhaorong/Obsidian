## 介绍
在Qt集成浏览器中，多数情况下，我们并不需要完整的浏览器界面，只需要一个渲染引擎以显示网页，并支持基本的 JavaScript 交互，有几种方式可以选择，`QtWebEngine`，`CEF`和`QCefView`。

### QtWebEngine
QtWebEngine 是 Qt 框架中的一个模块，用于在应用程序中集成现代 Web 技术。它基于 Chromium 开源项目，提供了一个用于显示 Web 内容的浏览器引擎，使开发者能够在他们的 Qt 应用程序中嵌入 Web 内容和功能。QtWebEngine 提供了一个易于使用的 API，开发者可以使用它来创建具有 Web 功能的应用程序，如浏览器、HTML5 游戏、在线帮助系统等。QtWebEngine 允许开发者在应用程序中直接使用 HTML、CSS、JavaScript 等 Web 技术，同时提供了与 Qt 框架的无缝集成，使开发者能够轻松管理和控制 Web 内容的展示和行为。

但是，QtWebEngine 模块基于的 Chromium 版本比较老，并没有随着 Chromium 项目快速迭代。比如我们使用的 Qt 5.15.2 中的 QtWebEngine 模块使用的是 Chromium 78 版本，而最新的 Chromium 版本已经到了 130。大多数情况下我们不需要跟进最新版本，但如果应用程序所访问的网站使用了最新的前端技术，那么 QtWebEngine 可能会出现一些显示异常的问题。

虽然 QtWebEngine 和 Chromium 都是开源的，但这两个项目都相当庞大，要将 QtWebEngine 升级到最新的 Chromium 版本，难度和工作量都相当大。

此外，我们还需要注意，Qt 的一些组件，这其中就包括 QtWebEngine， 是不能应用在商业项目中的。如果要在产品中使用 QtWebEngine，需要获得 Qt 商业许可证。

### CEF
CEF (Chromium Embedded Framework) 是一个开源项目，它允许开发者在自己的应用程序中嵌入 Chromium 浏览器引擎。它基于 Google 的 Chromium 项目，提供了一个稳定的、高性能的、现代的浏览器引擎，开发者可以通过 CEF 将其集成到自己的桌面应用程序中。

CEF 的优势之一是它提供了灵活的自定义和扩展性。开发者可以通过添加自定义的 JavaScript 扩展或使用 Chromium 的内置 API 来扩展和定制浏览器的功能。此外，CEF 还提供了一套丰富的 API，用于控制浏览器的行为、处理用户输入、管理 Web 内容等方面。

和 QtWebEngine 不同之处在于，CEF 采用了滚动发布策略，其主要特点包括：

- 持续更新：CEF 通过持续不断地将 Chromium 的最新代码集成到其代码库中，实现持续更新。这意味着 CEF 的开发者可以随时访问和使用最新的 Chromium 特性和改进。
    
- 频繁发布：CEF 以较短的周期发布新版本，通常每周或每两周发布一个新版本。这使得开发者能够更快地获得最新的功能、性能改进和安全补丁。
    
- 版本稳定性：尽管采用了持续更新和频繁发布的策略，CEF 仍然会在每个版本中进行测试和验证，以确保版本的稳定性和可靠性。
    
- 向后兼容：CEF 的滚动发布策略通常会保持向后兼容性，即在新版本中引入的改进和功能不会破坏现有的应用程序或功能。
    

在 QT 应用程序中集成 CEF，可以获得较新的 Chromium 内核，升级 Chromium 版本也相对容易一些。

### QCefView
QCefView 是一个与 CEF 集成的 Qt 小部件（Widget），可以使用 QCefView 而无需编写任何与 CEF 代码相关的代码，就像使用其它 Qt Widget 那样。项目采用了 LGPL-2.1 许可，可以放心在商业软件中免费使用。

项目地址：https://github.com/CefView/QCefView

中文文档：https://cefview.github.io/QCefView/zh/

## 构建 QCefView
需要按照[[Qt全平台开发搭建]]搭建好对应平台的Qt环境


### Window

准备：
Qt版本为6.7，支持MSVC2022的编译器，然后将MSVC2022的编译器添加到系统变量中，变量名设置为`QTDIR`
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-29-21-546126931f3acf56e2b236507a5ee88a-20241119142921-d64a81.png)


1. 下载Visual Studio
官网链接：https://visualstudio.microsoft.com/zh-hans/

在下载的Visual Studio Installer中，安装Visual Studio Community，注意部件的选择以及安装路径的更改
![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-23-17-f9d2961e5cc43a8ae26131adcb6717f0-20241119142316-516e27.png)

![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-23-35-2588bcd1d8c0cf363f35826af92b5313-20241119142334-0617a7.png)
部件需要选择`使用C++的桌面开发`和`Visual Studio扩展开发`，然后安装

2. 下载Cmake
官网链接：[CMake - Upgrade Your Software Build System](https://cmake.org/)

安装最新版即可

3. 开始编译
新建文件夹`CefView`，分别clone `CefViewCore`和 `QCefView`
```make
git clone https://github.com/CefView/CefViewCore.git
git clone https://github.com/CefView/QCefView.git
```
然后将`CefViewCore`移动到`QCefView`的目录下
![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-30-46-fb79dddac24161976a76ff0a8b42fb21-20241119143046-3d85de.png)


运行CMake gui，将`CMakeLists.txt`拖到Cmake中去
![image.png|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-31-34-c7159c619dcc5914e89aed903097cd40-20241119143133-ad86e4.png)
在`QCefView`中新建`build`目录，将CMake中的`Where to build the binaries`修改到`build`路径下
![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-34-18-cbdaf173c8d8492bedd5eb74579e2b40-20241119143417-6aab27.png)

点击`Configure`，选择`Visual Studio 2022 x64`,然后点击Finish
![image.png|725](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-36-21-2640712b75b103ec748461d1368614ac-20241119143620-3e1da2.png)

点击Configure，配置完之后再点击一次进行更新
![image.png|675](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-41-44-9000c352bb7fc466fb42b4968b9836ff-20241119144143-e0e6ed.png)
然后选择安装路径
![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-43-14-28e5b8caea2c6301062fb2ab04a5c938-20241119144313-74a14e.png)
点击`Generate`,然后点击`Open Project`打开Visual Studio,选择`CMakePredefinedTargets`的`ALL BUILD`右击点击生成
![image.png|459](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-44-24-3ef6fe9c67634716551a2573189d02fc-20241119144424-84bf90.png)

等待构建完毕
![image.png|1250](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-47-44-a59679ce3d2d693eb0b84e6fc8bcdda3-20241119144743-53d759.png)

再右击`INSTALL`，点击生成
![image.png|345](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-48-34-442caa9e28b7ca6f1b9189485e8f75c6-20241119144833-59d0c5.png)

生成在`D:\Qt\CefView\QCefView\bin\Debug`下

### Linux
1. 安装工具包和依赖包
```bash
sudo apt install libasound2 libasound2 g++ cmake 
```

2. 下载源码

```bash
git clone https://github.com/CefView/CefViewCore.git
git clone https://github.com/CefView/QCefView.git
```
将`CefViewCore`移动到`QCefView`目录下
```bash
mv -r CefViewCore QCefView/
```

3. 编译
需要选择对应的编译平台
```bash
cd QCefView
./generate-linux-x86_64.sh
cmake --build .build/linux.x86_64
```

安装后的库在当前目录的`.build/`下


## QtCreator导入QCefView
### Window
打开Qt项目，右击项目选择添加库
![image.png|510](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-51-29-2d85a893dce88b83d2e0df8657617c3f-20241119145128-021a8d.png)

选择外部库
![image.png|553](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-51-53-4473acecacf869249a3c50694feb8f0b-20241119145152-4636d6.png)

库文件选择`D:\Qt\CefView\QCefView\lib\Debug\QCefView.lib`
![|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-53-40-5b1e9b1b55654bc108270fb8279a4f56-20241119145339-50ebaf.png)、

头文件选择`D:\Qt\CefView\QCefView\include`
![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-54-44-37048fd5d68da819c00f19df74033c38-20241119145444-6d107e.png)

paltform选择window，点击下一步完成，即可导入
![image.png|575](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/14-55-53-2aa92a752e0521c713a8e6c743a42255-20241119145553-c3ea8e.png)

```bash
win32:CONFIG(release, debug|release): LIBS += -L$$PWD/../../../Qt/CefView/QCefView/lib/release/ -lQCefView
else:win32:CONFIG(debug, debug|release): LIBS += -L$$PWD/../../../Qt/CefView/QCefView/lib/debug/ -lQCefView

INCLUDEPATH += $$PWD/../../../Qt/CefView/QCefView/include
DEPENDPATH += $$PWD/../../../Qt/CefView/QCefView/include
```

### Linux

打开qtreator，库文件路径选择`/home/xuezhaorong/Share/QCefView/.build/linux.x86_64/output/Release/bin`下的`libQCefView`
![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/16-34-57-b99fa097c11bc04fc622dc8a59a0b974-20241119163456-c1f011.png)

头文件选择`/home/xuezhaorong/Share/QCefView/include`
![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/16-38-27-2ec4d3deaa2d2b77bcbafc0257e7e984-20241119163826-29e4fe.png)


