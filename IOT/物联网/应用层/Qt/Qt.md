## GUI程序基础 
### 主程序文件

```c++
#include <QApplication>  
#include <QWidget> 
int main(int argc, char *argv[]) {  
    QApplication a(argc, argv);  //定义并创建应用程序
    Widget w;  //定义并创建窗口对象
    w.show();  //显示窗口
    return QApplication::exec();  //运行应用程序，开始应用程序的消息循环和事件处理
}
```
	main()函数是 C++程序的入口。它的主要功能是定义并创建应用程序，定义并创建窗口对象和显示窗口，运行应用程序，开始应用程序的消息循环和事件处理。 QApplication 是 Qt 的标准应用程序类，main()函数里的第一行代码定义了一个 QApplication，它就是应用程序对象。然后定义了一个 Widget 类型的变量 w，Widget 是本示例设计的窗口的类名称，定义变量 w 就是创建窗口对象，然后用 w.show()显示此窗口。函数里最后一行用 a.exec()启动应用程序，开始应用程序的消息循环和事件处理。GUI 应用程序是事件驱动的，窗口上的各种组件接收鼠标或键盘的操作，并进行相应的处理。