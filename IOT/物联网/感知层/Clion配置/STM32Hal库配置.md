工具包链接：https://www.123pan.com/s/zum7Vv-sunPH.html
模板链接：https://wwl.lanzn.com/i22iY224uw1c

1. 安装JAVA，若已经安装，则不需要安装
![微信截图_20240701203237.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-32-56-3252b1b801db837b21957fe8471afeec-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701203237-cfadb8.png)

2. 安装STM32Cube，选择路径安装即可
![微信截图_20240701203352.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-34-02-953fd16612821bdc805cae69e73eb80e-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701203352-bcea79.png)

3. 解压其他3个文件夹
![微信截图_20240701203434.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-35-29-318928fcda7369832e63b2d519fd4831-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701203434-9fb2ff.png)

![微信截图_20240701203617.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-36-41-a6a4c394382f9326f0a5203240faa3f7-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701203617-17c072.png)

4. 设置环境变量
![微信截图_20240701203757.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-38-33-0482007ac823d6e18f769a44f6a0900d-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701203757-1fda10.png)

5. 设置CLion的嵌入式工具
![微信截图_20240701204019.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-40-28-e2c6ad8a6bb6937d2e1421f0967e55f3-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701204019-e3f072.png)

6. 配置构建工具链
![微信截图_20240701204533.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-46-35-29aaeaf7e2bc0247e355da3e20a20048-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701204533-9fc5f8.png)


PS：以上只用配置一次

PS: 下面可以直接使用模板

7. 新建一个STM32项目
![微信截图_20240701204241.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-42-51-4c0625a0039ceac2cf0aa634a5ee733a-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701204241-e8dad7.png)

8. 点击通过STM32Cube打开
![微信截图_20240701204728.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-47-35-6f400ab4191a6c7dd4d11de42b6df726-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701204728-f14173.png)

9. 点击芯片型号
![微信截图_20240701204817.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-48-24-715b494e72d5acddabd2f18c31f54e32-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701204817-888684.png)

10. 选择对应的芯片型号
![微信截图_20240701204916.png|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-49-29-0788da81d007bf62d0710969e931084a-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701204916-443b57.png)

11. 将时钟源选择为外部高速时钟
![image.png|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/01/22-32-17-b0c3e9f2f66382604789b8fd5e4d698d-20240701223217-879900.png)

12. 在sys中debug选择为对应调试器的接口类型
![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/01/22-34-27-1d569a18a1e562c33cf5755ef982edfc-20240701223426-bb2d1e.png)

13. 选择时钟源，将基准频率调为72MHZ
![|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/01/22-35-29-30727db7b7796ec772433c778bcabe0e-20240701223528-a9e0e3.png)

14. 点击ProjectManager，输入项目名称和选择IDE类型,项目名称要与Clion的文件名一致
![微信截图_20240701205041.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-50-50-73724f3f334ef8e320cc4c1d84124742-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701205041-78ba01.png)

13. 点击生成代码
![微信截图_20240701205238.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-52-46-c481fe91fe189dfb5aa6149ecebc3329-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701205238-6aab69.png)

14. 退出STM32Cube，返回Clion,如果出现面板配置文件，直接跳过
![微信截图_20240701205310.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-53-19-2c5d0f0cb808d66187b4b8a50e913897-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701205310-ec4795.png)

15. stlink配置
在项目文件中新建config文件夹，进入文件夹，新建stlink.cfg
![微信截图_20240701205825.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-58-43-a6422f06981ed0e02cec1de72b4de474-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701205825-baabcd.png)

16. 记事本打开stlink，写入配置代码
![微信截图_20240701205934.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/20-59-39-cd29f7e21e2a68621c828a84b3a3898f-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701205934-547c05.png)

```
# choose st-link/j-link/dap-link etc.
#adapter driver cmsis-dap
#transport select swd
source [find interface/stlink.cfg]
transport select hla_swd
source [find target/stm32f1x.cfg]
# download speed = 10MHz
adapter speed 10000
```

17. 在Clion中添加一个OpenOCD下载配置，并选择stlink.cfg
![微信截图_20240701205955.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-00-01-0fcdfbd547cc3cbf2143137b2da0bdae-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701205955-57fe13.png)


![微信截图_20240701210229.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-02-37-6e325b27016e5915370b9be12fb18639-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701210229-a76694.png)

![微信截图_20240701210255.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-03-04-446a5d4c7e96f5d8daa95d85b0fff3d4-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701210255-458f72.png)

18. 编译，然后点击运行，即可将程序烧录入单片机中，也可以进行单步调试
![微信截图_20240701210428.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-04-34-c614239e64e84ad8488d2dddc660772a-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701210428-ef404c.png)


PS:
* STM32Cube固件的安装路径更改，Help->Uptate Setting
![微信截图_20240701210518.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-05-24-094ab7101c3512cd46f57442904d84d4-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701210518-179a4b.png)

![微信截图_20240701210613.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-06-20-209260cc8c89b97aefe3a17984a21722-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701210613-38e104.png)

* STM32Cube开启系统代理
![微信截图_20240701210637.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-06-46-8a17461e5f263d81d34fd4affc4ca8e0-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701210637-180063.png)


* STM32Cube固件下载
![微信截图_20240701210715.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/07/01/21-07-20-6d085f87b83d3ed825b2cb1706813de5-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240701210715-e7d497.png)
* 查看外设寄存器
在STM32CubeMX软件中Help->Docs and Resources中找到System View Description，保存svd文件
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/01/21-12-07-939be3deea666e1bac6823c10ae53d48-20240701211207-4635e4.png)

在调试页面->Peripherials里面载入svd文件
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/01/21-13-04-66f019737f23fe950cda8bef2e944575-20240701211304-4715ab.png)

可以查看外设寄存器的值
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/01/21-14-04-9b389b3cb9d05b68c84f7a10c449f426-20240701211404-f7ba9c.png)

* 当项目名更改或地址变更时，只需要重新CMake，再编译即可,注意路径不可有中文!!!

* 解决printf重定向问题：
开启USEART1
![image.png|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/16-03-50-03fe0c60a694b514206b2c9912a9f976-20240904160349-87fdfa.png)

在项目中新建`retarget.h`文件，写入
```c
#include "stm32f4xx_hal.h"  
  
#include <sys/stat.h>  
  
#include <stdio.h>  
  
void RetargetInit(UART_HandleTypeDef *huart);  
  
int _isatty(int fd);  
  
int _write(int fd, char *ptr, int len);  
  
int _close(int fd);  
  
int _lseek(int fd, int ptr, int dir);  
  
int _read(int fd, char *ptr, int len);  
  
int _fstat(int fd, struct stat *st);
```

新建`retarget.c`文件，写入
```c  
#include "retarget.h"  
#include <_ansi.h>  
#include <_syslist.h>  
#include <errno.h>  
#include <sys/time.h>  
#include <sys/times.h>  
#include <retarget.h>  
#include <stdint.h>  
#if !defined(OS_USE_SEMIHOSTING)  
#define STDIN_FILENO 0  
#define STDOUT_FILENO 1  
#define STDERR_FILENO 2  
  
UART_HandleTypeDef *gHuart;  
  
void RetargetInit(UART_HandleTypeDef *huart){  
      gHuart = huart;  
      /* Disable I/O buffering for STDOUT stream, so that  
  * chars are sent out as soon as they are printed. */      setvbuf(stdout, NULL, _IONBF, 0);  
}  
  
int _isatty(int fd){  
  
      if (fd >= STDIN_FILENO && fd <= STDERR_FILENO)  
        return 1;  
      errno = EBADF;  
      return 0;  
}  
  
int _write(int fd, char *ptr, int len){  
      HAL_StatusTypeDef hstatus;  
      if (fd == STDOUT_FILENO || fd == STDERR_FILENO){  
            hstatus = HAL_UART_Transmit(gHuart, (uint8_t *) ptr, len, HAL_MAX_DELAY);  
            if (hstatus == HAL_OK)  
              return len;  
            else  
              return EIO;  
      }  
      errno = EBADF;  
      return -1;  
}  
  
  
  
int _close(int fd){  
      if (fd >= STDIN_FILENO && fd <= STDERR_FILENO)  
        return 0;  
      errno = EBADF;  
      return -1;  
}  
  
int _lseek(int fd, int ptr, int dir){  
      (void) fd;  
      (void) ptr;  
      (void) dir;  
      errno = EBADF;  
      return -1;  
}  
  
int _read(int fd, char *ptr, int len){  
      HAL_StatusTypeDef hstatus;  
      if (fd == STDIN_FILENO){  
            hstatus = HAL_UART_Receive(gHuart, (uint8_t *) ptr, 1, HAL_MAX_DELAY);  
            if (hstatus == HAL_OK)  
              return 1;  
            else  
              return EIO;  
      }  
      errno = EBADF;  
      return -1;  
}  
  
int _fstat(int fd, struct stat *st){  
      if (fd >= STDIN_FILENO && fd <= STDERR_FILENO){  
            st->st_mode = S_IFCHR;  
            return 0;  
      }  
      errno = EBADF;  
      return 0;  
}  
  
#endif //#if !defined(OS_USE_SEMIHOSTING)
```

然后cmake，编译，会出现报错
需要到`syscalls.c`文件中注释掉重定义的函数
![image.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/16-09-04-36dafd3044ae4371ea7037ddd1cf886c-20240904160904-3d5a0e.png)

最后在`main.c`中引入`retarget.h`头文件，在main函数中初始化，就可以使用`printf`重定向输出
```c
RetargetInit(&huart1);
```

