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

### 窗口类
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
        QWidget(parent), ui(new Ui::widget) {  
    ui->setupUi(this);    
}  
  
widget::~widget() {  
    delete ui;  
}
```

Widget 类目前只有构造函数和析构函数。其中构造函数头部语句如下： Widget::Widget(QWidget *parent): QWidget(parent), ui(new Ui::Widget) 这行代码的功能是运行父类 QWidget 的构造函数，创建一个 Ui::Widget 类的对象 ui。这个 ui 就是 Widget 类的 private 部分定义的指针变量 ui。构造函数里只有一行代码： ui->setupUi(this); 它表示运行了 Ui::Widget 类的 setupUi()函数，并且以 this 作为函数 setupUi()的输入参数，而 this 就是 Widget 类对象的实例，也就是一个窗口。setupUi()函数里会创建窗口上所有的界面组件， 并且以 Widget 窗口作为所有组件的父容器。所以，在文件 ui_widget.h 里有一个名字空间 Ui，里面有一个类 Widget，记作 Ui::Widget，它是窗口 UI 类。文件 widget.h 里的类 Widget 是完整的窗口类。在 Widget 类里访问 Ui::Widget 类的成员变量或函数需要通过 Widget 类里的指针 ui 来实现，例如构造函数里运行的 ui-> setupUi(this)。

### 封装窗口类
当Qt现存的窗口类无法满足需求时，则可以选择继承封装重写窗口类，流程如下：
1. 新建类，可以根据需求选择新建UI类或者cpp类（不需要UI或者由代码分成UI）
2. 在头文件中引入要继承类的头文件
```cpp
#include<QMainWindow>
```
3. 修改头文件中的继承类名
```cpp
#ifndef PROJECT_LOGINWIDGET_H  
#define PROJECT_LOGINWIDGET_H  
  
#include <QMainWindow>  
#include "ui_widget.h"  
  
QT_BEGIN_NAMESPACE  
namespace Ui { class widget; }  
QT_END_NAMESPACE  
  
class widget : public QMainWindow {  
Q_OBJECT  
  
public:  
    explicit widget(QWidget *parent = NULL);  
  
    ~LoginWidget();  
  
private:  
    Ui::widget *ui;  
};  
  
  
#endif //PROJECT_LOGINWIDGET_H
```

4. cpp中的构造函数修改
```cpp
#include "widget.h"  
  
widget::widget(QWidget *parent) :  
        QMainWindow(parent), ui(new Ui::widget) {  
    ui->setupUi(this);    
}  
  
widget::~widget() {  
    delete ui;  
}
```
如果只需要Cpp类需要删除.h文件中引入的ui头文件以及cpp中构造函数中的`ui(new Ui::widget)`和`ui->setupUi(this)`

### 信号与槽

要使用信号槽，必须在类的定义中包含Q_OBJECT宏。Q_OBJECT宏使得类能够使用信号和槽机制。在编译过程中，Qt的元对象编译器（moc）会读取带有Q_OBJECT宏的类的定义，并生成额外的代码来支持信号和槽的连接和调用。

元对象编译器（moc）：moc是一个工具，它读取带有Q_OBJECT宏的源文件，并生成一个额外的源文件，这个文件包含了元对象代码，包括信号和槽的实现。

* 信号（signal）是在特定情况下被发射的通知，例如 QPushButton 较常见的信号就是点击鼠标时发射的 clicked()信号。GUI 程序设计的主要工作就是对界面上各组件的信号进行响应，只需要知道什么情况下发射哪些信号，合理地去响应和处理这些信号就可以了。
* 槽（slot）是对信号进行响应的函数。槽就是函数，所以也称为槽函数。槽函数与一般的 C++ 函数一样，可以具有任何参数，也可以被直接调用。槽函数与一般的函数不同的是：槽函数可以与信号关联，当信号被发射时，关联的槽函数被自动运行。信号与槽关联是用函数 QObject::connect()实现的，使用函数 connect()的基本格式有如下几种：

**Qt4写法:**
```cpp
connect(ui->btnOpnen, SIGNAL(clicked), this, SLOT(open()));
```
- ==不推荐这种写法==，如果SIGNAL写错了，或者信号名字、槽函数名字写错了，编译器检查不出来，导致程序无响应，引起不必要的误解。

**Qt5写法:**
```cpp
connect(ui.btnOpnen, &QPushButton::clicked, this, &Widget::open);
```
==推荐使用这种写法==，信号名字、槽函数名字写错了，编辑器会直接报错。

**lambda表达式写法:**
```cpp
connect(ui.btnOpnen, &QPushButton::clicked, [=](){ //具体代码 });
```
适用于slot代码（槽函数代码）比较少，以及要获取到父类对象的情况。


C++ lambda表达式的本质就是重载了operator()，lambda表达式会被编译器翻译成类进行处理，在调用时会进行编译展开，因此lambda表达式对象其实就是一个匿名的函数对象，所以lambda表达式也叫做匿名函数对象。==Qt槽函数可以使用lambda函数来写==。
#### C++ lambda表达式的构成**：
```cpp
[caputer list] (parameters) mutable throw() -> return type{ statement }
```
* caputer list是捕获列表

	用于捕获外部变量，捕获的变量可以在函数体中使用，可以省略，即不捕获外部变量。一共有三种捕获方式：值捕获、引用捕获和隐式捕获。

* parameters是参数列表

	与普通函数的参数列表一致。如果不需要参数传递，则可以连同括号“()”一起省略，即无参数列表

* mutable关键字

	默认情况下Lambda函数总是一个const函数，mutable可以取消其常量性。加上mutable关键字后，可以修改传递进来的拷贝，在使用该修饰符时，参数列表不可省略（即使参数为空）。

* throw是异常说明

	用于Lamdba表达式内部函数抛出异常。

* return type是返回值类型。

	追踪返回类型形式声明函数的返回类型。我们可以在不需要返回值的时候也可以连同符号”->”一起省略。此外，在返回类型明确的情况下，也可以省略该部分，让编译器对返回类型进行推导。

* statement是Lambda表达式的函数体

	内容与普通函数一样，不过除了可以使用参数之外，还可以使用所有捕获的变量。

注意：可以忽略参数列表和返回类型，但必须永远包含捕获列表和函数体

下面介绍三种捕获方式并举例

* 值捕获：不能在lambda表达式中修改捕获变量的值

	值捕获意味着捕获的变量通过值传递给lambda表达式，即在lambda表达式中创建了这些变量的副本。因此，lambda表达式中使用的是这些变量的副本，而不是原始变量本身。由于被捕获变量的值是在创建时拷贝，因此随后对其求改不会影响到 lambda 内对应的值。

* 引用捕获：使用引用捕获一个外部变量，需要在捕获列表变量前面加上一个引用说明符&

	引用捕获意味着捕获的变量通过引用传递给lambda表达式，即lambda表达式中使用的是原始变量的引用，这意味着可以在lambda内部修改这些变量的值。

* 隐式捕获

	=为值捕获
	&为引用捕获
	
当我们混合使用了隐式捕获和显式捕获时，捕获列表中的第一个元素必须是一个 & 或 =。此符号指定了默认捕获方式为引用或值。

####  跨UI的信号槽
可以使用emit主动发送信号，实例[[Qt实例#界面跳转]]
```cpp
emit signal_();
```

## 本地存储
### 文件操作
