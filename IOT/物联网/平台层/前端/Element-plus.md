官方链接：[一个 Vue 3 UI 框架 | Element Plus](https://element-plus.org/zh-CN/)

## 安装
```
npm install element-plus --save
```

## 导入
```js
// main.ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```