## 布局
[[Qt#GUI程序基础]]
以制作登录界面为例，了解布局
1. 添加QUI类
![image.png|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-02-26-c3aed694ff5f800cde7956e790a67b88-20240725150226-4f7580.png)
将生成的.h，.cpp，.ui三个文件添加include，src和form目录中，并且**重新cmake（当添加，删除和移动文件时都要重新cmake）**。
2. 使用qt designer工具进行ui布局
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-03-58-f575a53377b9dbf76b1d63a3514d0d6e-20240725150357-15ef08.png)
3. 调整父窗口的尺寸
![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-08-34-b52a4fc1e1174c8ed4fb2ba4a2328398-20240725150834-db8966.png)

4. 在左边的工具栏中拖入**label**和**lineEdit**控件，框选全部控件，点击上方的栅栏布局，进行快速对齐。
![3352d84abad3d5c6ccf112b6a7053f2.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/FileStorage/Temp/2024/07/25/15-07-35-79ba7b984f2c60e251c17c11dfb242f8-3352d84abad3d5c6ccf112b6a7053f2-d05ef6.png)
5. 控件之间可以通过添加水平弹簧调整间距，通过调整栅栏布局以及控件的minisize调整控件大小
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-11-29-7db1549e5cb790fffccf6a62feaf022e-20240725151128-cb8920.png)
6. 重命名控件名称
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-56-31-144dcb48175c6861aa8d75803635d695-20240725155629-b2f9b3.png)

7.  调整完成后，保存退出，选择UIC工具生成ui.h文件
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-12-07-221ee4b75acc273074f29a10d74d861e-20240725151207-806e0b.png)
8. 在**main.cpp**中引入登录界面类的头文件，实例化，调用**show**方法显示界面窗口
```c++
#include <QApplication>  
#include <QDebug>  
#include "loginwidget.h"  
int main(int argc, char *argv[]) {  
    QApplication a(argc, argv);  
    qDebug() << "Start Project";  
    LoginWidget l;  
    l.show();  
    return QApplication::exec();  
}
```

## 登录响应
[[Qt#信号与槽]]
在登录界面上添加登录退出按钮并添加功能，了解信号与槽机制，只介绍关键内容，其他的具体操作可看上节。
1. 在登录界面通过qt designer新建两个按钮控件，登录以及退出，布局并且改变对象名称。
![image.png|875](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/17-00-31-048ba762eac4b9877f55981dfecbf361-20240725170030-e6f7e7.png)
2. 为两个按钮添加槽函数，优先使用lambda的形式，检测按钮的按下信号，触发响应的槽函数。
```cpp
connect(ui->pushButton_exit, &QPushButton::clicked,[=](){  
    close();  //  close方法 退出
});  
  
connect(ui->pushButton_login,&QPushButton::clicked,[=](){  
   QString username = ui->lineEdit_username->text();  
   QString pwd = ui->lineEdit_pwd->text();  
   if(username=="xuezhaorong" && pwd=="123456" ){  
       // 登录成功
   }  
  
});
```

## 界面跳转
新建一个menu类，演示页面跳转的逻辑，要实现登录界面-菜单界面的双向跳转，则登录界面为父类，承载着菜单界面子类，在销毁或隐藏子类之前，父类有且只有一个，且不能销毁，所以父类跳转到子类只需要触发条件后实例化子类对象并且调用show方法跳转，自身可以选择隐藏但不能销毁，而子类返回父类则需要通过发送信号的方法通知父类，而不是新建一个新的父类跳转，这样不是返回而是进入到一个新的界面中，导致内存冗杂，头文件互相包含以及数据错误等问题。
1. 新建一个菜单界面menu，加入返回按钮并且重命名。
2. 在menu.h中定义自定义信号`void signal_menu_return();`在menu.cpp中添加信号槽，实现按下按钮发送自定义信号，并且调用close方法关闭自身。
```cpp
// .h
signals:  
    void signal_menu_return();
```

```cpp
// .cpp
connect(ui->pushButton_return,&QPushButton::clicked,[=](){  
   emit signal_menu_return();  
   this->close();  
});
```

3. 在login.cpp中添加点击登录按钮验证成功后跳出menu界面以及自身隐藏的业务代码，实例化menu时，需要用new指针的方法创建在堆区，以保证生命周期以及调用qt的ui自动回收机制，调用close方法之后回收内存，然后为其绑定槽函数，以便自身重新show出来。
```cpp
// 登录按钮  
connect(ui->pushButton_login,&QPushButton::clicked,[=](){  
   QString username = ui->lineEdit_username->text();  
   QString pwd = ui->lineEdit_pwd->text();  
   if(username=="xuezhaorong" && pwd=="123456" ){  
       menu *m = new menu();  
       m->show();     
       connect(m,&menu::signal_menu_return,[=](){  
           this->show();  
       });  
       this->hide();  
   }  
});
```

## 文件操作
	[[Qt#读写文件]]
1. 添加注册按钮，实现点击注册，打开对应文件（没有则创建一个），写入注册信息（按符号分隔的账号和密码）。
2. 首先需要读取文件，按行查找当前注册的信息是否注册过，避免重复注册，新增读取文件函数`bool openByIO_Lines(const QString &aFileName,QString mUsername,QString mPwd);`，参数为注册文件的路径，要注册的用户名和密码，使用`split`方法分割一行里面的账号和密码，因为密码后面跟着是`\n`换行符，所以在判断时要加一个`\n`换行符号。
```cpp
bool LoginWidget::openByIO_Lines(const QString &aFileName,QString mUsername,QString mPwd){ // 逐行读取  
    QFile aFile;  
    aFile.setFileName(aFileName);  
    if (!aFile.exists()) //文件不存在  
         return false;  
    if (!aFile.open(QIODevice::ReadOnly | QIODevice::Text)) // 以只读，文本方式打开文件  
        return false;  
  
    while (!aFile.atEnd()) {  
        QByteArray line = aFile.readLine(); //读取一行文字，自动添加“\0”  
        QString str= QString::fromUtf8(line); //从字节数组转换为字符串，文件必须采用 UTF-8 编码  
        QStringList strList = str.split("|"); // 分割字符串  
        QString username = strList[0];  
        QString pwd = strList[1];  
        if(username == mUsername && pwd == mPwd+"\n") {  
            aFile.close();  
            return true;  
        }  
    }  
    aFile.close(); // 关闭文件  
    return false;  
}
```
3. 接着是写入注册信息的函数`bool saveByIO_Whole(const QString &aFileName,QString mUsername,QString mPwd);`。
```cpp
bool LoginWidget::saveByIO_Whole(const QString &aFileName,QString mUsername,QString mPwd){  
    QFile aFile(aFileName);  
    if (!aFile.open(QIODevice::Append | QIODevice::Text)) // 只写文本方式打开  
        return false;  
  
    QString str = mUsername + "|" + mPwd + "\n";  
    QByteArray strBytes = str.toUtf8(); // 转换成字节数组  
  
    aFile.write(strBytes,strBytes.length());  
    aFile.close();  
    return true;  
}
```
4. 整合两个函数，为注册按钮触发绑定信号槽。
```cpp
// 注册按钮  
connect(ui->pushButton_register,&QPushButton::clicked,[=](){  
    QString username = ui->lineEdit_username->text();  
    QString pwd = ui->lineEdit_pwd->text();  
    bool registerResult = openByIO_Lines(loginFiFo,username,pwd);  
    if (!registerResult) // 未注册  
        saveByIO_Whole(loginFiFo,username,pwd);  
  
});
```
5. 登录按钮时，改为读取注册文件，比较当前的账号密码。
```cpp
// 登录按钮  
connect(ui->pushButton_login,&QPushButton::clicked,[=](){  
   QString username = ui->lineEdit_username->text();  
   QString pwd = ui->lineEdit_pwd->text();  
   bool loginResult = openByIO_Lines(loginFiFo,username,pwd);  
   if(loginResult){ // 登录成功  
       menu *m = new menu();  
       m->show();  
       connect(m,&menu::signal_menu_return,[=](){  
           this->show();  
       });  
       this->hide();  
   }  
});
```

## 事件处理

以虚拟键盘为例，掌握事件处理的过程，包括重写事件和事件过滤，要求点击编辑框时跳出虚拟键盘。
1. 新建`frmKeyBoard`类，设计UI界面，布局并且更改对象名称。
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/26/20-56-02-25d1574b76caeb2806762f6709938351-20240726205601-73827c.png)
2. `frmKeyBoard`类需要继承`Dialog`类进行重写[[Qt#封装窗口类]]。
3. 作为虚拟键盘，不能影响主窗口的焦点，需要设置成无焦点窗口，同时隐藏标题框。
```cpp
this->setWindowFlags(Qt::WindowStaysOnTopHint | Qt::FramelessWindowHint | Qt::WindowDoesNotAcceptFocus);  
this->setWindowTitle("屏幕键盘");  
this->setWindowModality(Qt::WindowModal);  
this->setAttribute(Qt::WA_DeleteOnClose);
```
4. 要实现点击按钮，在主窗口编辑框输出的效果，需要在按按钮时手动发送事件信息，这样事件信息就会被主窗口获取触发对应按钮事件
	`sendEvent`对象为主窗口中的聚焦对象`m_focusWidget->focusWidget()`如编辑框等，一开始为按钮设置重复发送的功能方便长按连续输出，同时为按钮取消焦点，防止`sendEvent`对象转移。
```cpp
void frmKeyBoard::initFrm(){  
    // 找到所有的按钮  
    QList<QPushButton*> pbtns = this->findChildren<QPushButton*>();  
    foreach(QPushButton * pbtn, pbtns) {  
        pbtn->setAutoRepeat(true);    //允许自动重复  
        pbtn->setAutoRepeatDelay(500);//设置重复操作的时延  
        pbtn->setFocusPolicy(Qt::NoFocus);  
  
  
        if (pbtn->text() >= 'a' && pbtn->text() <= 'z') {  // 字母
            connect(pbtn, &QPushButton::clicked, [=](){  
                QKeyEvent keyPress(QEvent::KeyPress, pbtn->text().toInt() + 48, Qt::NoModifier, pbtn->text());  
                QKeyEvent keyRelease(QEvent::KeyRelease, pbtn->text().toInt() + 48, Qt::NoModifier, pbtn->text());  
                QApplication::sendEvent(m_focusWidget->focusWidget(), &keyPress);  
                QApplication::sendEvent(m_focusWidget->focusWidget(), &keyRelease);  
            });  
            letterBtnVec.push_back(pbtn);  
        }  
        else if (pbtn->text().toInt() > 0 && pbtn->text().toInt() <= 9 || pbtn->text() == "0") {  // 数字
            connect(pbtn, &QPushButton::clicked, [=](){  
  
                QKeyEvent keyPress(QEvent::KeyPress, pbtn->text().toInt() + 48, Qt::NoModifier, pbtn->text());  
                QKeyEvent keyRelease(QEvent::KeyRelease, pbtn->text().toInt() + 48, Qt::NoModifier, pbtn->text());  
                QApplication::sendEvent(m_focusWidget->focusWidget(), &keyPress);  
                QApplication::sendEvent(m_focusWidget->focusWidget(), &keyRelease);  
            });  
  
        }  
        else{  
            if(pbtn == ui->pushButton_closeKeyboard){ // 退出按钮  
                connect(pbtn,&QPushButton::clicked,[=](){  
                   this->close();  
                   emit signal_emit_close();  
                });  
            }  
            else if(pbtn == ui->pushButton_backspace){  // 删除按钮
                connect(pbtn,&QPushButton::clicked,[=](){  
                    QKeyEvent keyPress(QEvent::KeyPress, Qt::Key_Backspace, Qt::NoModifier);  
                    QKeyEvent keyRelease(QEvent::KeyRelease, Qt::Key_Backspace, Qt::NoModifier);  
                    QApplication::sendEvent(m_focusWidget->focusWidget(), &keyPress);  
                    QApplication::sendEvent(m_focusWidget->focusWidget(), &keyRelease);  
                });  
            }  
            else if(pbtn == ui->pushButton_sapce){  // 空格按钮
                connect(pbtn,&QPushButton::clicked,[=](){  
                    QKeyEvent keyPress(QEvent::KeyPress, Qt::Key_Space, Qt::NoModifier, " ");  
                    QKeyEvent keyRelease(QEvent::KeyRelease, Qt::Key_Space, Qt::NoModifier, " ");  
                    QApplication::sendEvent(m_focusWidget->focusWidget(), &keyPress);  
                    QApplication::sendEvent(m_focusWidget->focusWidget(), &keyRelease);  
                });  
            }  
            else if(pbtn == ui->pushButton_capslk){  // 大小写转换
  
                connect(pbtn,&QPushButton::clicked,[=](){  
                    static bool capFlag = false;  
                    capFlag = !capFlag;  
                    if (capFlag) {  
  
                            foreach(QPushButton * letterBtn,letterBtnVec){  
                                letterBtn->setText(letterBtn->text().toUpper());  
                            }  
                    }  
                    else {  
                            foreach(QPushButton * letterBtn,letterBtnVec){  
                                letterBtn->setText(letterBtn->text().toLower());  
                            }  
                    }  
                });  
            }  
        }  
    }  
  
}```

5. 要实现点击编辑框弹出虚拟键盘窗口的功能，原始的方式则是监测其发出的信号，绑定对应的槽函数，但是编辑框控件的点击信号并不完全，无法完全实现功能，**面对控件信号无法实现功能的问题有两种方式可以解决**：1. 使用事件过滤器，获取更丰富的事件（信号其实就是事件的封装），进行处理;2. 直接重新封装控件，自定义功能，前面的虚拟键盘则是采用这种方式。这里采用第一种方法。
* 首先为编辑框安装事件过滤器：
```cpp
ui->lineEdit_username->installEventFilter(this);  
ui->lineEdit_pwd->installEventFilter(this);
```

* 重写事件过滤器，在实例化虚拟键盘时，将自身传入，设为父对象，同时使用`setFocus`方法为编辑框设置焦点。
```cpp
bool LoginWidget::eventFilter(QObject *watched, QEvent *event){  
    if((watched == ui->lineEdit_username || watched == ui->lineEdit_pwd) && event->type() == QEvent::MouseButtonPress){  
        QLineEdit *mlineEdit = (QLineEdit *)watched;  
        if(f != NULL){  
            delete f;  
            f = NULL;  
        }else{  
            f = new frmKeyBoard(this);  
            mlineEdit->setFocus();  
  
            connect(f,&frmKeyBoard::signal_emit_close,[=](){  
                delete f;  
                f = NULL;  
            });  
            f->show();  
        }  
    }  
  
    return QWidget::eventFilter(watched,event);  
}
```

* 另外还需要为虚拟键盘实现可以移动的功能，可以重写`void mousePressEvent(QMouseEvent* event)`和`void mouseMoveEvent(QMouseEvent* event)`方法来通过计算移动和点击按钮之间的偏移量来实现移动。
```cpp
void frmKeyBoard::mousePressEvent(QMouseEvent* event){  
    m_dragPosition = event->pos();  
  
    QDialog::mousePressEvent(event);  
}  
  
void frmKeyBoard::mouseMoveEvent(QMouseEvent* event){  
  
    QPoint currentPoint = mapTo(m_focusWidget,QPoint(0,0));  // 将点在窗口的坐标转换为在对象上的坐标
  
    QPoint movePoint = currentPoint + (event->pos() - m_dragPosition);  
    move(movePoint);  
  
    QDialog::mouseMoveEvent(event);  
}
```

## 多线程
[[Qt#多线程]]
以传输视频图像为例，掌握多线程，原理是获取摄像头的画面，转换为QImage并显示在label上面，由于需要不断执行这个过程，所以要放在子线程中执行，防止阻碍主线程。
[[安装qt5]]在树莓派中安装qtcreator，并且配置好opencv[[Qt交叉编译配置#opencv交叉编译]]，使用vnc连接树莓派
将Clion中的文件移植到qtcreator中去[[Clion配置Qt#移植]]
新增capThread线程类，继承QThread

![image.png|650](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/14-28-57-9f5117493d3477f3dd2acfe4a8950db4-20240803142857-4fa098.png)
![image.png|650](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/14-29-33-2447cfeb7065c5ec97f98897628ae4b1-20240803142932-8dddf9.png)

定义并打开摄像头
```cpp
VideoCapture cap;
cap.open(0);
```

重写run函数不断发送获取的图片
```bash
void CapThread::run(){
    while(cap.isOpened()){ // 判断摄像头是否打开
        cap >> frame; // 摄像头输出到Mat frame
        Mat framel;
        cvtColor(frame, framel, cv::COLOR_RGB2BGR); // 由于cap获取的Mat颜色通道为GBR，要将其化为RGB

        QImage img = QImage((const unsigned char *)(framel.data),
                  framel.cols, framel.rows,
                  framel.step, QImage::Format_RGB888); // 将Mat加载到Qimage中
        QImage resizeImg = img.scaled(mlabelSize, Qt::KeepAspectRatio); // 进行大小调整，Qt::KeepAspectRatio表示裁剪时按比例缩小扩大不改变原来的大小
        if (!frame.empty()){
            emit signal_emit_capFrame(resizeImg); // 发送信号
        }
        msleep(30);
    }
}
```

主线程中则获取不断传回来的画面，并显示在label上面
```bash
connect(capThread,&CapThread::signal_emit_capFrame,[=](QImage img){
	ui->label_cap->setPixmap(QPixmap::fromImage(img));
	ui->label_cap->show();
});
```