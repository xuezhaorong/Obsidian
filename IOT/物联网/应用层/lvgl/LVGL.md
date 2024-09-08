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
