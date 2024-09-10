开发文档链接：[欢迎阅读LVGL中文开发手册！ — LVGL 文档 (100ask.net)](https://lvgl.100ask.net/master/)
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
可在[Style properties（样式属性） — LVGL 文档 (100ask.net)](https://lvgl.100ask.net/master/overview/style-props.html#size-and-position)查询
![image.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/16-56-46-52a177e2954136c09458fc1934b8b4c4-20240908165645-ad5fe5.png)

示例代码：
```c
lv_obj_t * obj = lv_obj_create(lv_scr_act());  
lv_obj_set_align(obj,LV_ALIGN_CENTER);  
  
lv_obj_set_style_border_color(obj, lv_color_hex(0x56c94c),LV_STATE_DEFAULT);  // 设置边框颜色
lv_obj_set_style_border_width(obj,10,LV_STATE_DEFAULT);  // 设置边框宽度
lv_obj_set_style_border_opa(obj,200,LV_STATE_DEFAULT); // 设置边框透明度

lv_obj_set_style_outline_color(obj, lv_color_hex(0x0000a0),LV_STATE_DEFAULT);  // 轮廓
lv_obj_set_style_outline_width(obj,10,LV_STATE_DEFAULT);  // 论据
lv_obj_set_style_outline_opa(obj,200,LV_STATE_DEFAULT); // 轮廓
```

现象： 
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/17-06-54-5ed87d85afab860974be1caab47796e2-20240908170653-7d7a4e.png)

打开[Widgets（控件） — LVGL 文档 (100ask.net)](https://lvgl.100ask.net/master/widgets/index.html)，查看修改部件中各个部分的属性

![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/17-25-02-0e26dbe29ee7ec1f94ba2ba3ef24038a-20240908172502-2e06c8.png)

示例代码：
```c
lv_obj_t *slider = lv_slider_create(lv_scr_act());  
lv_obj_set_align(slider,LV_ALIGN_CENTER);  
  
lv_obj_set_style_bg_color(slider, lv_color_hex(0x94bc40),LV_STATE_DEFAULT|LV_PART_INDICATOR );  // 设置指示器颜色
lv_obj_set_style_bg_color(slider, lv_color_hex(0x94bc40),LV_STATE_DEFAULT|LV_PART_KNOB ); // 设置手柄颜色
```


现象：
![image.png|725](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/17-29-14-41ac2b5c986f1c843fb48dfddd262438-20240908172914-fbe838.png)





#### 事件(events)
当发生用户感兴趣的事件时，可以触发事件，以执行相应的操作
Api函数：
* 添加事件：`lv_obj_add_event_cb(obj,event_cb,event_code,user_data)`
* 删除事件：`lv_obj_remove_event_cb(obj,event_cb)`

到[Events（事件） — LVGL 文档 (100ask.net)](https://lvgl.100ask.net/master/overview/event.html)查看

示例代码：
```c
static void my_event_cb(lv_event_t *e){  
    printf("LV_EVENT_CLICKED \n");  
}  
  
void my_gui(void){  
  
    lv_obj_t *obj = lv_obj_create(lv_scr_act());  
    lv_obj_add_event_cb(obj,my_event_cb,LV_EVENT_CLICKED,NULL); 
}
```

如果多个事件触发使用同一个回调函数，可以使用函数`lv_event_get_code`进行区分
示例代码：
```c
lv_event_code_t code = lv_event_get_code(e);  
if (code == LV_EVENT_CLICKED){  
    printf("LV_EVENT_CLICKED \n");  
}  
else if (code == LV_EVENT_PRESSED){  
    printf("LV_EVENT_PRESSED \n");  
}
```

如果多个部件使用同一个回调函数，可以使用函数`lv_event_get_target`区分
示例代码：
```c  
lv_obj_t *obj = NULL;  
lv_obj_t *obj2 = NULL;  
  
static void my_event_cb(lv_event_t *e){  
    lv_obj_t *target = lv_event_get_target(e);  
    if (target == obj){  
        printf("obj \n");  
  
    }  
    else if (target == obj2){  
        printf("obj2 \n");  
  
    }  
  
}  
  
void my_gui(void){  
  
    obj = lv_obj_create(lv_scr_act());  
    lv_obj_add_event_cb(obj,my_event_cb,LV_EVENT_CLICKED,NULL);  
    lv_obj_set_size(obj,300,300);  
  
    obj2 = lv_obj_create(lv_scr_act());  
    lv_obj_add_event_cb(obj2,my_event_cb,LV_EVENT_PRESSED,NULL);  
  
}
```

## 部件
### 标签(lv_label)
#### 创建标签部件
```c
lv_obj_t *label = lv_label_create(parent);
```

#### 设置文本
```c
lv_label_set_text(label,"hello lvgl");
lv_label_set_text_fmt(label,"hello %s","lvgl");
```

#### 设置样式
* 背景颜色：
```c
lv_obj_set_style_bg_color(label, lv_color_hex(0xff0000),LV_STATE_DEFAULT);
```

* 透明度：
```c
lv_obj_set_style_bg_opa(label,100,LV_STATE_DEFAULT);
```

* 字体大小：
```c
lv_obj_set_style_text_font(label,&lv_font_montserrat_30,LV_STATE_DEFAULT);
```
需要在`lv_conf.h`中开启对应的字体，将0设为1
![image.png|331](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/09/11-57-24-0f9b237ef308db813d056eeb96180169-20240909115723-a27957.png)

* 字体颜色：
```c
lv_obj_set_style_text_color(label,lv_color_hex(0x5084db),LV_STATE_DEFAULT);
```

* 设置个别字体的字体颜色：
```c
lv_label_set_recolor(label,true);  // 开启重写着色模式
lv_label_set_text(label,"hello #ff0000 lvgl#");
```

#### 长文本
* 默认情况下，如果没有限定标签部件的大小，那它的大小自动扩展为文本大小
* 长文本模式：
```c
lv_label_set_long_mode(label,LV_LABEL_LONG_...);
```

```c
enum {  
    LV_LABEL_LONG_WRAP,    // 默认模式，如果部件大小已固定，超出的文本将被剪切  
    LV_LABEL_LONG_DOT,     // 将label右下角的最后三个字符替换为点  
    LV_LABEL_LONG_SCROLL,  // 来回滚动  
    LV_LABEL_LONG_SCROLL_CIRCULAR,  // 循环滚动 
    LV_LABEL_LONG_CLIP,    // 直接剪切部件外面的文本部分
};
```

### 按钮(lv_btn)
#### 创建按钮部件
```c
lv_obj_t *btn = lv_btn_create(lv_scr_act());
```

#### 设置样式
* 设置大小
```c
lv_obj_set_size(btn,100,50);  
```

* 设置样式
```c
lv_obj_set_align(btn,LV_ALIGN_CENTER);  
```

* 添加事件
```c
lv_obj_set_style_bg_color(btn, lv_color_hex(0xffe1d4),LV_STATE_PRESSED);
```

#### 添加事件
```c
lv_obj_add_flag(btn,LV_OBJ_FLAG_CHECKABLE);  // 开启状态切换
lv_obj_add_event_cb(btn,my_event_cb,LV_EVENT_VALUE_CHANGED,NULL);
```

### 开关(lv_switch)
开关部件常用于控制某个功能的开启和关闭，它可以直接显示被控对象的状态

#### 创建开关部件
```c
lv_obj_t *switch1 = lv_switch_create(lv_scr_act());  
lv_obj_set_style_bg_color(switch1, lv_color_hex(0xdf5345),LV_STATE_CHECKED | LV_PART_INDICATOR); // 设置手柄颜色，与上状态检测，避免被主题颜色覆盖
```

#### 添加，清除开关状态
```c
lv_obj_add_state(switch1,LV_STATE_CHECKED | LV_STATE_DISABLED);  // 添加状态，默认打开且不能修改
lv_obj_clear_state(switch1,LV_STATE_CHECKED); // 清除开关的状态
```