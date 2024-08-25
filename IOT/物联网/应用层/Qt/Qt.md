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

要使用信号槽，必须在类的定义中包含Q_OBJECT宏。Q_OBJECT宏使得类能够使用信号和槽机制。
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
	若需要获取参数，则在括号中写入参数，如(QImage img)

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
实例[[Qt实例#文件操作]]
QFile 主要的功能是进行文件读写，它可以读写文本文件或二进制文件。用 QFile 进行文件内容读写的基本操作步骤是：
（1）调用函数 open()打开或创建文件；
（2）用读写函数读写文件内容；
（3）调用函数 close()关闭文件。调用 close()会将缓存的数据写入文件，如果不能正常调用 close()，可能会导致文件数据丢失。
#### 创建QFile类和绑定文件路径
QFile提供了两种方式：
*  先创建QFile类，然后使用`setFileName`方法绑定文件路径
```cpp
QFile aFile;  
aFile.setFileName(aFileName);
```

*  创建QFile类的同时绑定文件路径：
```cpp
QFile aFile(aFileName);
```

#### 打开文件
QFile 的函数 open()的原型定义如下：
`bool QFile::open(QIODeviceBase::OpenMode mode)`
参数 mode 决定了文件以什么模式打开，mode 是标志类型 `QIODeviceBase::OpenMode`，它是枚举类型`QIODeviceBase::OpenModeFlag` 的枚举值的组合，其各主要枚举值的含义如下：
1. QIODevice::ReadOnly：以只读模式打开文件，加载文件时使用此模式。 
2. QIODevice::WriteOnly：以只写模式打开文件，保存文件时使用此模式。 
3. QIODevice::ReadWrite：以读写模式打开文件。 
4. QIODevice::Append：以添加模式打开文件，新写入文件的数据添加到文件尾部。
5. QIODevice::Truncate：以截取模式打开文件，文件原有的内容全部被删除。 
6. QIODevice::Text：以文本模式打开文件，读取时“\n”被自动翻译为一行的结束符，写入时字符串结束符会被自动翻译为系统平台的编码，如 Windows 平台上是“\r\n”。
在给函数 open()传递参数时，可以使用枚举值的组合，例如 QIODevice::ReadOnly | QIODevice::Text 表示以只读和文本模式打开文件。
* 在读文件时，需要判断文件是否存在，再使用`open`打开文件。
```cpp
if (!aFile.exists()) //文件不存在  
         return false;  
    if (!aFile.open(QIODevice::ReadOnly | QIODevice::Text)) // 以只读，文本方式打开文件  
        return false;  
```

* 在写文件时，直接以写文本方式打开文件。
```cpp
    if (!aFile.open(QIODevice::WriteOnly | QIODevice::Text)) // 只写文本方式打开  
        return false;  
```

#### 读写文件
在读写文件时，读出的是`QByteArray`类型，要转换成QString类型的要使用`QString::fromUtf8`方法，写文件时，要写入`QByteArray`类型，所以要使用`toUtf8()`方法将`QString`类型转换。
* 读文件，分为全部读取和按行读取
```cpp
bool MainWindow::openByIO_Whole(const QString &aFileName) {//整体读取 
	QFile aFile(aFileName); 
	if (!aFile.exists()) //文件不存在 
		return false; 
	if (!aFile.open(QIODevice::ReadOnly | QIODevice::Text)) 
			return false; QByteArray all_Lines = aFile.readAll(); //读取全部内容 
	QString text(all_Lines); //将字节数组转换为字符串 
	aFile.close();
	return true; 
	
}
```

```cpp
bool MainWindow::openByIO_Lines(const QString &aFileName) {//逐行读取 
	QFile aFile; 
	aFile.setFileName(aFileName); 
	if (!aFile.exists()) //文件不存在 
		return false; 
	if (!aFile.open(QIODevice::ReadOnly | QIODevice::Text)) 
		return false; 
	while (!aFile.atEnd()) { 
		QByteArray line = aFile.readLine(); //读取一行文字，自动添加“\0” 
		QString str= QString::fromUtf8(line); //从字节数组转换为字符串，文件必须采用 UTF-8 编码 
		str.truncate(str.length()-1); //去除增加的空行 
	}
	aFile.close();  
	return true; 
}
```

* 写文件
```cpp
bool MainWindow::saveByIO_Whole(const QString &aFileName) { 
	QFile aFile(aFileName); 
	if (!aFile.open(QIODevice::WriteOnly | QIODevice::Text)) 
	return false; 
	
	QByteArray strBytes= str.toUtf8(); //转换为字节数组，UTF-8 编码 
	aFile.write(strBytes,strBytes.length()); //写入文件 
	aFile.close(); 
	return true; 
}
```

## 事件系统
窗口系统是由事件驱动的，Qt为事件处理编程提供了完善的支持。QWidget类是所有界面组 件类的基类，QWidget类定义了大量与事件处理相关的数据类型和接口函数。
### 事件处理
任何从QObject 派生的类都可以处理事件，但其中主要是从QWidget派生的窗口类和界面组 件类需要处理事件，因为大多数事件都是通过界面操作产生的，例如鼠标事件、按键事件等。 一个类接收到应用程序派发来的事件后，首先会由函数event()处理。event()是 QObject 类中 定义的一个虚函数，其函数原型定义如下：
```cpp
bool QObject::event(QEvent *e)
```
其中，参数e是事件对象，通过`e->type()`就可以得到事件的具体类型。 任何从QObject 派生的类都可以重新实现函数`event()`，以便在接收到事件时进行处理。如果 一个类重新实现了函数event()，需要在函数event()的实现代码里设置是否接受事件。QEvent类有 两个函数，函数 accept()接受事件，表示事件接收者会对事件进行处理；函数 ignore()忽略事件， 表示事件接收者不接受此事件。被接受的事件由事件接收者处理，被忽略的事件则传播到事件接 收者的父容器组件，由父容器组件的event()函数去处理，这称为事件的传播（propagation），事件 最后可能会传播给窗口。
以鼠标移动事件为例，演示如何处理事件[[Qt实例#事件处理]]。
1. 在头文件的`protected`域中声明该函数:
```cpp
void mousePressEvent(QMouseEvent* event);
```
 2. 在源文件中实现对应功能，加入处理业务代码后，可以选择给基类处理事件以正常响应：
```cpp
void frmKeyBoard::mousePressEvent(QMouseEvent* event){  
    m_dragPosition = event->pos();  
  
    QDialog::mousePressEvent(event);  
}
```

### 事件过滤器
如果要对一个标签的某些事件进行处理，需要重新定义一个标签类，在标签类里重新实现函数event()或对应的事件处理函数。 QObject 还提供了另一种处理事件的方法：事件过滤器。它可以将一个对象的事件委托给另 一个对象来监视并处理。例如，一个窗口可以作为其界面上的QLabel组件的事件过滤器，派发给 QLabel 组件的事件由窗口去处理，这样，就不需要为了处理某种事件而新定义一个标签类。
要实现事件过滤器功能，需要完成两项操作。 
（1）被监视对象使用函数`installEventFilter()`将自己注册给监视对象，监视对象就是事件过滤器。 
（2）监视对象重新实现函数`eventFilter()`，对监视到的事件进行处理。
如要实现点击编辑框响应的功能：
1. 在Widget中为编辑框安装事件过滤器
```cpp
ui->lineEdit_username->installEventFilter(this);  
```
2. 重写`eventFilter()`函数，可以根据`wathed`判断事件的对象，`event->type()` 判断事件类型
```cpp
bool LoginWidget::eventFilter(QObject *watched, QEvent *event){  
    if(watched == ui->lineEdit_username  && event->type() == QEvent::MouseButtonPress){  

  
    return QWidget::eventFilter(watched,event);  
}
```

## 多线程
一个 QThread 类的对象管理一个线程。在设计多线程程序的时候，需要从 QThread 继承定义线程类，并重定义 QThread 的虚函数 run()，在函数 run()里处理线程的事件循环。我们把应用程序的线程称为主线程，创建的其他线程称为工作线程。一般会在主线程里创建工作线程，并调用函数 start()开始执行工作线程的任务。函数 start()会在其内部调用函数 run()进入工作线程的事件循环，函数 run()的程序体一般是一个无限循环，可以在函数 run()里调用函数 exit() 或 quit()结束线程的事件循环，或在主线程里调用函数 terminate()强制结束线程。
#### QThread
QThread 的父类是 QObject，所以可以使用信号与槽机制。QThread 自身定义了 started()和 finished()两个信号，started()信号在线程开始运行之前，也就是在函数 run()被调用之前被发射； finished()信号在线程即将结束时被发射。

要实现QThread类，需要创建继承QThread的类。
`.h`
```cpp
#include <QThread>
#include <QObject>
class CapThread : public QThread
{
    Q_OBJECT
public:
    CapThread(QObject *parent = NULL);
    ~CapThread();
protected:
    void run();
};
```

`.cpp`
```cpp
CapThread::CapThread(QObject *parent):QThread(parent)
{

}


void CapThread::run(){

}
```
必须重定义函数 run()，线程的任务就在这个函数里实现。

#### 线程同步
在多线程程序中，由于存在多个线程，线程之间可能需要访问同一个变量，或一个线程需要等待另一个线程完成某个操作后才产生相应的动作。为了避免出现当前线程中在访问和操作资源时，另一个线程也对其进行访问和操作而造成冲突，就需要用到线程同步的技术。
QMutex 和 QMutexLocker 是基于互斥量（mutex）的线程同步类，QMutex 定义的实例是互斥量，QMutex 主要有以下几个函数。
```cpp
void QMutex::lock() //锁定互斥量，一直等待 
void QMutex::unlock() //解锁互斥量 
bool QMutex::tryLock() //尝试锁定互斥量，不等待 
bool QMutex::tryLock(int timeout) //尝试锁定互斥量，最多等待 timeout 毫秒
```

* 函数 lock()锁定互斥量，如果另一个线程锁定了这个互斥量，它将被阻塞运行直到其他线程解锁这个互斥量。
* 函数 unlock()解锁互斥量，需要与 lock()配对使用。
* 函数 tryLock()尝试锁定一个互斥量，如果成功锁定就返回 true，如果其他线程已经锁定了这个互斥量就返回 false，不等待。
* 函数 tryLock(int timeout)尝试锁定一个互斥量，如果这个互斥量被其他线程锁定，最多等待 timeout 毫秒。

互斥量相当于一把钥匙，如果两个线程要访问同一个共享资源，就需要通过 lock()或 tryLock()拿到这把钥匙，然后才可以访问该共享资源，访问完之后还要通过 unlock()还回钥匙，这样别的线程才有机会拿到钥匙。
基本操作如下，在需要不断访问的临界资源前后加入锁与解锁，在触发访问的临界资源前后加上尝试锁和解锁[[Qt实例#多线程]]
```cpp
void CapThread::slot_recordStop(){
    if(mutex_recordFlag.tryLock(100)){
        mrecordFlag = false;
        capWriter.release();
        mutex_recordFlag.unlock();
    }

}

void CapThread::run(){
    while(cap.isOpened()){
        mutex_recordFlag.lock();
        if(mrecordFlag){
            capWriter << framel;
        }
        mutex_recordFlag.unlock();

        msleep(30);
    }
}
```


## 音频输入输出
