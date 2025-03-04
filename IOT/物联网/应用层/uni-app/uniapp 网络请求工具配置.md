> 官网链接：https://www.npmjs.com/package/@escook/request-miniprogram

## 安装
```shell
npm install @escook/request-miniprogram
```

## 导入
在`main.js`的`#ifdef VUE3`中写入
```js
// 按需导入 $http 对象
import { $http } from '@escook/request-miniprogram'

uni.$http = $http
```

## 使用
### 支持的请求方法
```js
// 发起 GET 请求，data 是可选的参数对象
$http.get(url, data?)

// 发起 POST 请求，data 是可选的参数对象
$http.post(url, data?)

// 发起 PUT 请求，data 是可选的参数对象
$http.put(url, data?)

// 发起 DELETE 请求，data 是可选的参数对象
$http.delete(url, data?)
```

### 配置请求根路径
```js
$http.baseUrl = 'https://www.example.com'
```

### 请求拦截器
```js
// 请求开始之前做一些事情
$http.beforeRequest = function (options) {
  // do somethimg...
}
```

### 响应拦截器
```js
// 请求完成之后做一些事情
$http.afterRequest = function () {
  // do something...
}
```