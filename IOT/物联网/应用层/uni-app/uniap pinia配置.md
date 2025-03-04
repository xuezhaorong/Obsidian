> 官方配置链接：https://pinia.vuejs.org/zh/getting-started.html
## 安装
```shell
npm install pinia
```
## 导入
在`main.js`的`#ifdef VUE3`中添加
```js
import {
  createPinia
} from 'pinia'

export function createApp() {
...
  const pinia = createPinia()
  app.use(pinia)
  
}

```

## 使用方法
具体的使用方法参照[[Pinia]]