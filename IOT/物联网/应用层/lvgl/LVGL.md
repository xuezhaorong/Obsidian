## 基础对象
### 基础对象简介
LVGL 采用的是面向对象的编程思想，以抽象的类来实例化不同的对象
![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/15-34-57-601a059534f8321587c04a5b69b84f33-20240908153457-a0afdc.png)

示例代码：创建一个开关
```c
lv_obj_t * switch_obj = lv_switch_create(lv_scr_act()); // 以活动屏幕为父对象
lv_obj_t * switch_obj2 = lv_switch_create(switch_obj); // 以开关为父对象
```

基础对象（lv_obj_t）可以作为父对象，来创建其他对象，同时它也可以作为部件使用
![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/15-44-16-d6e040ae8a6dba72baeafe9ad90a5bf4-20240908154415-524af3.png)


### 父和子对象的关系
* 子对象会随着父对象移动

示例代码：
```c
lv_obj_t *obj1 = lv_obj_create(lv_scr_act());  
lv_obj_set_size(obj1,300,400);  
lv_obj_set_pos(obj1,20,20);  
lv_obj_t *obj2 = lv_obj_create(obj1);
```

现象：
![image.png|675](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/15-48-45-cfb58a9a98725ef178a7e1695024c602-20240908154845-749fe2.png)

* 子对象的位置超出父对象的范围，则超出的部分不显示
示例代码：
```c
lv_obj_t *obj1 = lv_obj_create(lv_scr_act());  
lv_obj_set_size(obj1,300,400);  
lv_obj_set_pos(obj1,20,20);  
  
lv_obj_t *obj2 = lv_obj_create(obj1);  
lv_obj_set_pos(obj2,400,20);
```

现象：
![image.png|725](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/15-50-51-c5c264144bc6597f4ceac85ddb593b86-20240908155051-bff3a5.png)

### 部件的基本属性
#### 大小(size)
Api函数：
* 设置宽度：`lv_obj_set_width(obj,new_width)`
* 设置高度：`lv_obj_height(obj,new_height)`
* 同时设置宽度，高度：`lv_obj_set_size(obj,new_width,new_height)`
示例代码：
```c
lv_obj_t *obj1 = lv_obj_create(lv_scr_act());  
//    lv_obj_set_width(obj1,300);  
//    lv_obj_set_height(obj1,300);  
lv_obj_set_size(obj1,300,400);
```

#### 位置(position)

![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-03-30-35d3607e49871aceb1f2d010258b85cb-20240908160330-034980.png)

 设置部件位置位置时，坐标原点在父对象的左上角
![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-01-44-af63fb0addaad50a061dd56816db56d4-20240908160143-35db80.png)

Api函数
* 设置X轴坐标：`lv_obj_set_x(obj,new_x)`
* 设置Y轴坐标：`lv_obj_set_y(obj,new_y)`
* 同时设置X，Y轴坐标：`lv_obj_set_pos(obj,new_x,new_y)`

示例代码：
```c
lv_obj_t *obj1 = lv_obj_create(lv_scr_act());  
//    lv_obj_set_x(obj1,100);  
//    lv_obj_set_y(obj1,200);  
lv_obj_set_pos(obj1,100,200);
```

#### 对齐(alignment)
对齐方式有两种
1. 参照父对象对齐
![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-14-43-35d012202dc91ed96892b8d060e046af-20240908161442-273e37.png)

Api函数：
* 参照父对象对齐：`lv_obj_set_align(obj,LV_ALIGN_...)`
* 参照父对象对齐，再进行偏移：`lv_obj_align(obj,lv_ALIGN_...,x,y)` 

示例代码：
```c
lv_obj_t *obj1 = lv_obj_create(lv_scr_act());  
lv_obj_set_align(obj1,LV_ALIGN_CENTER);
```

现象：
![|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-26-00-d67378cff5fe121b7eed7d3f229ce33f-20240908162559-12650b.png)


3. 参照其他对象对齐（无父子关系）
![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-15-28-de1357b4a919bbd46d0cd96a851582df-20240908161528-b1e0bc.png)

Api函数：
* 参照其他对象对齐（无父子关系），再进行偏移：`lv_obj_align_to(obj_to_align,obj_referece,LV_ALIGN_,...,x,y)`

 示例代码：
 ```c
lv_obj_t *obj1 = lv_obj_create(lv_scr_act());  
lv_obj_set_size(obj1,300,300);  
  
lv_obj_t *obj2 = lv_obj_create(lv_scr_act());  
lv_obj_align_to(obj2,obj1,LV_ALIGN_OUT_RIGHT_MID,0,0); 
```

现象：
![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-31-19-aa76b4c28d0644e79f7113df1d229d8f-20240908163119-87d9d3.png)


**对齐模式**：
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-22-39-1b79fbf4d6a13ab4fefb93daaf012a45-20240908162238-a51f92.png)

中间灰色部分是父子关系的对齐模式，父子关系其他模式不能选择

#### 样式(stytles)
样式用于设置部件的外观，以优化显示界面和实现用户交互
* 添加普通样式
```c
static lv_style_t style;  // 定义样式变量
lv_style_init(&style);  // 初始化样式
lv_style_set_bg_color(&style, lv_color_hex(0xf4b183));  // 设置背景颜色
  
lv_obj_t * obj = lv_obj_create(lv_scr_act());  // 创建一个部件
lv_obj_add_style(obj,&style,LV_STATE_DEFAULT); // 设置部件的样式
```

* 添加本地样式
```c
lv_obj_t * obj = lv_obj_create(lv_scr_act());  
lv_obj_set_style_bg_color(obj, lv_color_hex(0xf4b183),LV_STATE_DEFAULT);
```

`lv_obj_add_style`和`lv_obj_set_style_bg_color`的第三个参数为样式的选择状态，代表样式什么状态下生效
```c
enum {  
    LV_STATE_DEFAULT     =  0x0000,  // 默认状态
    LV_STATE_CHECKED     =  0x0001,  // 切换或选中状态
    LV_STATE_FOCUSED     =  0x0002,  // 通过键盘，编码器聚焦或通过触摸板，鼠标点击
    LV_STATE_FOCUS_KEY   =  0x0004,  // 通过键盘，编码器聚焦
    LV_STATE_EDITED      =  0x0008,  // 由编码器编辑
    LV_STATE_HOVERED     =  0x0010,  // 鼠标悬停（已不支持）
    LV_STATE_PRESSED     =  0x0020,  // 已按下
    LV_STATE_SCROLLED    =  0x0040,  // 滚动状态
    LV_STATE_DISABLED    =  0x0080,  // 禁用状态
};
```

可以设置的样式
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-52-11-af07eb8c44fe58c76312e1b6e8fc7da0-20240908165210-4e7e30.png)
