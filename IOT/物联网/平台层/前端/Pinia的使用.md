>官方网址：[简介 | Pinia](https://pinia.vuejs.org/zh/introduction.html)

## 导入
在`main.js`中导入
```js
import { createPinia } from 'pinia'
const pinia = createPinia();
app.use(pinia)
```

## 使用
在`src`下创建`store/store.js`，其中js文件为想要持久化的对象名称。

```js
import { defineStore } from "pinia";  

export const deviceStore = defineStore("device", {

  state: () => {

    return {

      count: 0,

    };

  },

  actions: {
	increment() { this.count++ }
	
  }
});
```

在`state`中定于要持久化的数据，`actions`中定于方法，要使用`this.`来引用`state`中的数据。