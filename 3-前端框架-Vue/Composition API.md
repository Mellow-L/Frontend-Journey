Composition API（组合式API），区别于以往OptionsAPI（选项式API）。
==区别==：
- 组织方式：
	- OptionsAPI要求变动 data，method，computed 三部分（选项）以实现一个功能或需求。
	- 新的Composition API按照功能逻辑组织代码。（可以将js代码写在一整个< script >当中）
- 逻辑复用：
	- Composition API更容易提取和复用逻辑。
- TS支持：
	- Composition API提供了更好的类型推导。



```js
<script setup lang="js"></script> // setup语法糖
```
setup-组合式API入口点

一个vue3组件的加载顺序：
1. 初始化
2. 执行setup
3. template编译与渲染
4. 生命周期钩子触发

vue项目运行顺序
index.html为入口文件，指向main.js或main.ts
```js
import {createApp} from "vue";
import App from "@/App.vue";
const app = createApp(App) --App为根组件
app.mount('#app') --mount 将根组件挂载到id为app的元素处
```

