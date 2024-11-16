The Network Security Services (NSS) package is a set of libraries designed to support cross-platform development of security-enabled client and server applications. Applications built with NSS can support SSL v2 and v3, TLS, PKCS #5, PKCS #7, PKCS #11, PKCS #12, S/MIME, X.509 v3 certificates, and other security standards. This is useful for implementing SSL and S/MIME or other Internet security standards into an application.

This package is known to build and work properly using an LFS-9.1 platform.
需要编译sqlite3和nspr，这里选择的是`NSS-3.50`,`NSPR-4.25`,`SQLite-3.31.1`，官方链接：https://www.linuxfromscratch.org/blfs/view/9.1/postlfs/nss.html

## 编译sqlite3
官方链接：https://www.linuxfromscratch.org/blfs/view/9.1/server/sqlite.html
下载对应的压缩包到树莓派中解压，切换到目录内，输入配置命令
```bash
./configure --prefix=/usr     \
            --disable-static  \
            --enable-fts5     \
            CFLAGS="-g -O2                    \
            -DSQLITE_ENABLE_FTS3=1            \
            -DSQLITE_ENABLE_FTS4=1            \
            -DSQLITE_ENABLE_COLUMN_METADATA=1 \
            -DSQLITE_ENABLE_UNLOCK_NOTIFY=1   \
            -DSQLITE_ENABLE_DBSTAT_VTAB=1     \
            -DSQLITE_SECURE_DELETE=1          \
            -DSQLITE_ENABLE_FTS3_TOKENIZER=1"
```

编译安装：
```bash
make
sudo make install
```

## 编译nspr
官方链接：https://www.linuxfromscratch.org/blfs/view/9.1/general/nspr.html
下载对应的压缩包到树莓派中解压，切换到目录内，输入命令更改规则
```bash
sed -ri 's#^(RELEASE_BINS =).*#\1#' pr/src/misc/Makefile.in
sed -i 's#$(LIBRARY) ##'            config/rules.mk
```

输入配置指令：
```bash
./configure --prefix=/usr \
            --with-mozilla \
            --with-pthreads \
            $([ $(uname -m) = x86_64 ] && echo --enable-64bit)
```

编译安装：
```bash
make
sudo make install
```

在继续编译nss时，需要更改一个头文件，避免编译错误
```bash
sudo vim /usr/include/nspr/prtypes.h
```

找到下面的代码片段(大概在555行，将近末尾)，进行更改
```c
#define PR_STATIC_ASSERT(condition) \
    extern void pr_static_assert(int arg[(condition) ? 1 : -1])
```

改为
```c
#define PR_STATIC_ASSERT(condition) \
    extern void pr_static_assert(int arg[(condition) ? 1 : 0])
```

## 编译nss
官方链接：https://www.linuxfromscratch.org/blfs/view/9.1/postlfs/nss.html
下载对应的压缩包到树莓派中解压，切换到第一层目录下(与nss同级)，创建文档`nss-3.50-standalone-1.patch`,写入官方的补丁
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/15/19-25-48-3de40e8b48a14bf0f1494f909474eae8-20241115192547-0775f8.png)

点开后是一堆代码，需要将其中的路径，换成`nss/Makefile`，使用编辑器的批量替换功能
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/15/19-27-07-2fd210c0e578d6d02df0afdfdaf6de8f-20241115192706-2a3840.png)

在写入文件后，切换到nss目录下，输入补丁命令：
```bash
patch -Np1 -i ../nss-3.50-standalone-1.patch
```
没有出现错误，代表成功

输入编译命令：
```bash
make -j1 BUILD_OPT=1                  \
  NSPR_INCLUDE_DIR=/usr/include/nspr  \
  USE_SYSTEM_ZLIB=1                   \
  ZLIB_LIBS=-lz                       \
  NSS_ENABLE_WERROR=0                 \
  $([ $(uname -m) = x86_64 ] && echo USE_64=1) \
  $([ -f /usr/include/sqlite3.h ] && echo NSS_USE_SYSTEM_SQLITE=1)
```

编译成功后，切换到`dist`目录下
```bash
cd ../dist
```
输入安装命令：
```bash
sudo install -v -m755 Linux*/lib/*.so              /usr/lib
sudo install -v -m644 Linux*/lib/{*.chk,libcrmf.a} /usr/lib
sudo install -v -m755 -d                           /usr/include/nss
sudo cp -v -RL {public,private}/nss/*              /usr/include/nss
sudo chmod -v 644                                  /usr/include/nss/*
sudo install -v -m755 Linux*/bin/{certutil,nss-config,pk12util} /usr/bin
sudo install -v -m644 Linux*/lib/pkgconfig/nss.pc  /usr/lib/pkgconfig
```

安装完毕