官方链接：https://www.axios-http.cn/docs/intro

## 安装
```bash
npm install axios
```

## 配置重定向
在`vite.config.js`的`return`中添加一个节点，`target`中为api地址
```js
server: {  
  proxy: {  
    "/api": {  
      target: "http://localhost:8041",  
      changeOrigin: true,  
      rewrite: (path) => path.replace(/^\/api/, ""),  
    },  
  },  
},
```

## 使用
1. 新建`utils`工具目录，新建`request.js`请求工具
```js
import axios from "axios";

const instance = axios.create({ baseURL: "/api" });

// 添加响应拦截器
instance.interceptors.response.use(
  (result) => {
    return result.data;
  },
  (err) => {
    alert("服务异常");
    return Promise.reject(err); // 异步状态转化为失败的状态
  }
);

export default instance;
```

在`api`目录中新建api文件
```js
// 导入
import request from "@/utils/request";
// get
export const get = () => {
  return request.get("");
};
```