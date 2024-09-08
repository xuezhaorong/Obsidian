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
子对象会随着父对象移动
```c

```