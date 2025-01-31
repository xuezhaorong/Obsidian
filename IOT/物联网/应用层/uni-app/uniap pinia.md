`HBuilder X` 已内置了 `Pinia`，无需手动安装

## 导入
新建目录`store`和初始化文件`index.js`
```js
import { createPinia } from 'pinia'
const store = createPinia()
export default store
```

在`main.js`中找到`createSSRApp`的方法,进行更改
```js
// #ifdef VUE3
import { createSSRApp } from 'vue'
// pinia
import store from './store'
export function createApp() {
  const app = createSSRApp(App)
  app.use(store)
  return {
    app
  }
}
// #endif
```

## 使用方法
具体的使用方法参照[[Pinia]]