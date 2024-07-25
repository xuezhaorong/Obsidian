## 布局
[[Qt#GUI程序基础]]
以制作登录界面为例，了解布局
1. 添加QUI类
![image.png|675](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-02-26-c3aed694ff5f800cde7956e790a67b88-20240725150226-4f7580.png)
将生成的.h，.cpp，.ui三个文件添加include，src和form目录中。
2. 使用qt designer工具进行ui布局
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-03-58-f575a53377b9dbf76b1d63a3514d0d6e-20240725150357-15ef08.png)
3. 调整父窗口的尺寸
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-08-34-b52a4fc1e1174c8ed4fb2ba4a2328398-20240725150834-db8966.png)

4. 在左边的工具栏中拖入**label**和**lineEdit**控件，框选全部控件，点击上方的栅栏布局，进行快速对齐。
![3352d84abad3d5c6ccf112b6a7053f2.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/FileStorage/Temp/2024/07/25/15-07-35-79ba7b984f2c60e251c17c11dfb242f8-3352d84abad3d5c6ccf112b6a7053f2-d05ef6.png)
5. 控件之间可以通过添加水平弹簧调整间距，通过调整栅栏布局以及控件的minisize调整控件大小
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-11-29-7db1549e5cb790fffccf6a62feaf022e-20240725151128-cb8920.png)
6. 重命名控件名称
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/15-56-31-144dcb48175c6861aa8d75803635d695-20240725155629-b2f9b3.png)

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
