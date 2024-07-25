## GUI程序基础 
### 主程序文件

```cpp
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

## 窗口类
一个窗口类由4个文件组成，刚开始创建时有.cpp，.h，.ui文件，ui.h文件则是由.ui文件通过UIC工具生成。
* widget.h： 定义了窗口类 Widget widget.cpp 实现 Widget 类的功能的源程序文件 
* widget.ui： 窗口 UI 文件，用于在 Qt Designer 中进行 UI 可视化设计。
* widget.ui： 是一个 XML 文件，存储界面上各个组件的属性和布局内容 
* ui_widget.h： UI 文件经过 UIC 编译后生成的文件，这个文件里定义了一个类，类的名称是 Ui_Widget，用 C++语言描述 UI 文件中界面组件的属性设置、布局以及信号与槽的关联等内容

**widget.h：**
```cpp
#ifndef PROJECT_LOGINWIDGET_H  
#define PROJECT_LOGINWIDGET_H  
  
#include <QWidget>  
#include "ui_widget.h"  
  
QT_BEGIN_NAMESPACE  
namespace Ui { class widget; }  
QT_END_NAMESPACE  
  
class widget : public QWidget {  
Q_OBJECT  
  
public:  
    explicit widget(QWidget *parent = NULL);  
  
    ~LoginWidget();  
  
private:  
    Ui::widget *ui;  
};  
  
  
#endif //PROJECT_LOGINWIDGET_H
```
1. 窗口 UI 类 Ui::Widget 的声明。文件中有如下几行代码： QT_BEGIN_NAMESPACE namespace Ui { class Widget; } //一个名字空间 Ui，包含一个类 Widget QT_END_NAMESPACE 上述代码声明了一个名称为 Ui 的名字空间（namespace），它包含一个类 Widget。
2. 窗口类 Widget 的定义。文件 widget.h 的主体部分是一个继承自 QWidget 的类 Widget 的定义。Widget 类中包含创建窗口界面，实现窗口上界面组件的交互操作，以及其他业务逻辑。 Widget 类定义的第一行语句插入了一个宏 Q_OBJECT，**这是使用 Qt 元对象系统的类时必须插入的一个宏。插入了这个宏之后，Widget 类中就可以使用信号与槽、属性等功能。** Widget 类的 private 部分定义了一个指针，代码如下： Ui::Widget *ui; 这个指针是用前面声明的名字空间 Ui 里的类 Widget 定义的，所以 ui 是窗口 UI 类的对象指针，它指向可视化设计的窗口界面。要访问界面上的组件，就需要通过这个指针来实现。
3.  在这个文件的开头部分自动加入了如下的一行包含语句： `#include "ui_widget.h"` 文件 ui_widget.h 是 UI 文件 widget.ui 被 UIC 编译后生成的文件，这个文件里定义了窗口 UI 类。

**widget.cpp**：
```cpp
#include "widget.h"  
  
widget::widget(QWidget *parent) :  
        QWidget(parent), ui(new Ui::LoginWidget) {  
    ui->setupUi(this);    
}  
  
widget::~widget() {  
    delete ui;  
}
```

