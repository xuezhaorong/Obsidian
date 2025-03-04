>官方链接：https://uiadmin.net/uview-plus/components/install.html
1. 新建一个项目
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/04/19-39-45-6d2cd0acc02d23f744f9156b86603592-20250304193943-679d69.png)

2. 进入该插件的应用市场安装到HBuilderX中，官方链接：https://ext.dcloud.net.cn/plugin?name=uview-plus
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/04/19-45-32-623a8491d05e1005b56a9f2174342463-20250304194531-4d320b.png)

3. 进行配置，官方配置链接：https://uiadmin.net/uview-plus/components/downloadSetting.html
	1. 引入uview-plus主JS库
		在项目根目录中的`main.js`中，引入并使用uview-plus的JS库，注意这两行要放在`const app = createSSRApp(App)`之后。
	```js
	// main.js
	import uviewPlus from '@/uni_modules/uview-plus'
	
	// #ifdef VUE3
	import { createSSRApp } from 'vue'
	export function createApp() {
	  const app = createSSRApp(App)
	  app.use(uviewPlus)
	  return {
	    app
	  }
	}
	// #endif
	```
	2. 在引入uview-plus的全局SCSS主题文件
		在项目根目录的`uni.scss`中引入此文件。
	```css
	/* uni.scss */
	@import '@/uni_modules/uview-plus/theme.scss';
	```
	3. 引入uview-plus基础样式
	```css
	<style lang="scss">
		/* 注意要写在第一行，注意不能引入至uni.scss，同时给style标签加入lang="scss"属性 */
		@import "@/uni_modules/uview-plus/index.scss";
	</style>
	```
	4. 安装依赖库
	```
	npm i dayjs
	npm i clipboard
	```
