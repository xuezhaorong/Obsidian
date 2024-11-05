## 保存自动格式化代码
ArtisiticStyle工具官网链接：https://astyle.sourceforge.net/
1. 加入`Beautidier`插件 
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-42-54-de52ad44f2382544adc182b72701b356-20241031124253-afc49f.png)
设置后重启Qt

2. 下载ArtisiticStyle工具
![image.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-43-39-40f0dd11096c986fd20f5d78bb9c09f9-20241031124338-c47d18.png)

3. 编译安装
```cmake
mkdir  as-gcc-exe
cd  as-gcc-exe
cmake  ../
make
make install 
```
注意：如果是Window操作系统直接下载可执行包:https://www.123684.com/s/zum7Vv-c86XH
4. 配置美化器
![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-45-39-943d092201c4851f05811dbe9bd0574e-20241031124538-b73a5d.png)

![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-46-53-44f86c2b96a3fd3140cba14fff2509dc-20241031124652-9a5c87.png)

添加样式
```
style=allman             # 设置 大括号 风格
indent=spaces=4          # 每个缩进使用 4 个空格
attach-closing-while     # 将“do-while”语句的结束“while”附加到结束大括号
indent-preproc-block     # 缩进预处理
indent-preproc-define    # 缩进以反斜杠结尾的多行预处理器定义
indent-col1-comments     # 从第一列开始缩进 C++ 注释
min-conditional-indent=1 # 设置在由多行构建标题时添加的最小缩进量
break-blocks             # 在标题块周围填充空行（例如“if”、“for”、“while”
pad-oper                 # 在运算符周围插入空格填充
pad-comma                # 在逗号后插入空格填充
```

