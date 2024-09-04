主体文件链接：https://wwl.lanzn.com/izHz51s0bmkd
模板链接：https://wwl.lanzn.com/iqWmx224uw6h

1.   创建项目和打开 STM32CubeMX 与普通的流程[[STM32Hal库配置]]一致
2.   在 STM32CubeMX 中设置 Freerots

![image](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111837054.png)

3.   为 SYS 设置一个不常用的 TIM

![img](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111845841.png)

4. 在`CMakeLists_template.txt`中取消注释浮点支持
![image.png|1200](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/22-26-04-04ec695a1a6958ac6c2652c243891e32-20240904222603-aa224c.png)

5. 其他配置与普通流程相同。
