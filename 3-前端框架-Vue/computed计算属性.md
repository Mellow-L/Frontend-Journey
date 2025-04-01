返回值
- 计算属性返回一个只读的ref对象，用.value取值。
- 若希望返回值可写：
	- 如下定义的fullName是可读可写的，若无set方法，则为只读属性。
```js
let fullName = computed({
	get(){
	
	},
	set(){
	
	}
})
```
