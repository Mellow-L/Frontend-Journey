1. watch方法：
```js
watch(sum,(newValue,oldValue)=>{

})
```
sum是被监视数据，后带一个发生变动时的回调函数。newValue和oldValue为sum的新旧值。
**被监视数据 为响应式数据**，否则将出现invalid watch source警告


2. watchEffect方法：
```js
watchEffect(()=>{})  
```
监视数据较多时可用。此方法自动捕捉内部相关响应式数据的变化。

3. 响应式数据：
- ref reactive定义响应式数据：
	- **基本类型-响应式数据，用ref**
	- **对象类型-响应式，层级不深，用ref，reactive均可**
	- **对象类型-响应式，层级深，用reactive****

- ==ref 可定义基本类型，对象类型数据。==
	- ref定义基本类型数据，需要用.value取值
	- ref定义对象类型数据，实际被监视的是对象地址值，在watch末尾加上`,{deep:true}`开启深度监视，即可监视对象内部属性。

- ==reactive 只能定义对象类型数据。==
	- 定义的对象默认开启深度监视。

- 响应式对象赋值：
	- 对reactive对象直接赋值将失去响应性，因为没有用reactive{ }包裹。
		- 需要使用`Object.assign(car, {brand:'xxxx',price:100}`为reactive对象批量更改新值。
		- 或者`car = reactive({ brand: 'xxxx', price: 100 });`
	- 而对于ref对象，直接赋值给.value，car仍为响应式：
		- `car.value = {brand:'xxxx',price:1000}`
	- 对响应式对象保持响应性的解构-- **toRefs 方法使 reactive 对象内部属性成为 ref 属性**（多个）
		- toRef针对单个属性。toRef 相反的有 toValue
		- `let person = reactive({...})`
		- `let{name, age} = toRefs(person)`将内部属性也转为响应式，实例代码如下：
	- 类似的还有storeToRefs 。（使store实例 内部state属性 转化为具有 `.value` 的 ref 对象集合  [[Pinia状态管理]]）
```js
import { reactive, toRefs } from 'vue';
const state = reactive({
  name: 'Alice',
  age: 25
});

const refs = toRefs(state);

console.log(refs.name.value); // 'Alice'
console.log(refs.age.value);  // 25

```

- 监视ref或reactive定义的对象类型内的某个属性或对象（对象套对象）
- **建议均将被监视数据写成函数形式**
```js
watch(()=>return person.name,()=>{// 实际将属性加工为getter函数

})

watch(()=>person.car,()=>{// 假设这里car是一个对象

})
```
- 监视多个数据
```js
watch([()=>return person.name, ()=>person.car],()=>{

})
```

4. 创建浅层响应式引用：ShallowRef
	- 如下，currentComponent为响应式，但shallowRef不触发内部深层次响应式处理，可以提高性能。
```js
const components = [
    AllArticles,
    MyArticles,
    MyComments,
    MyProfile,
]
const activeTab = ref(1)// 表示激活状态导航项的index
const currentComponent = shallowRef(components[activeTab.value])
```