## Qt
具体的使用方式：[[Qt mqtt]]
通过mqtt发送`json`数据
```cpp
mqttModel->MqttPublish(topic, messageJsonBytes);
```

## 后端
### 本地开发环境
本地开发需要本地的mqtt服务器保证编译的完成，选用`emqx`
#### 配置emqx
官网链接：[免费试用 EMQX Cloud 或 EMQX Enterprise | 下载 EMQX](https://www.emqx.com/zh/try?tab=self-managed)
下载安装开源版：
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/09/13-25-38-add604e091e5270f111548667046b1a7-20241209132537-81fbbc.png)

