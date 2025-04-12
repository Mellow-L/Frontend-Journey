- 方法说明：直接调用函数，并手动指定 `this`，**参数逐个传递**
- 用法：`fn.call(thisArg, arg1, arg2, ...)` 
	- thisArg即this指向
	- argn即多个传入参数
- 类似： `apply()` [[apply方法]]
- **手写Call**
	- 理解以下代码
		- 使用Symbol [[Symbol]]创建：
			- 使key成为唯一键名，避免与context中已有属性名冲突
		- `context[key]`：
			- 上下文是一个对象，将作为this
			- **key成为context对象的属性，临时的，用于挂载目标函数**
			- 实际上是一个函数
		- `context[key] = this`：
			- this在这里是调用myCall的函数的上下文
		- delete：
			- 删除临时添加的属性key
			- **执行完就清理**是模拟call的精髓之一
		- `const result = context[key](...args)`
			- 调用key属性，在这里key是一个函数
			- 传入参数args来执行它
			- result是执行后的返回
```js
Function.prototype.myCall = function(context,...args){
  context = context ?? globalThis
  const key = Symbol('tempFn')
  context[key] = this // 这里的 this 是调用 myCall 的那个函数本身
  const result = context[key](...args) // 传入参数，执行这个函数
  delete context[key]
  return result
}
```
- 关于上下文传递的理解：
	- `context[key]=this`将greet挂载到person上，成为person对象的一个方法
	- `context[key](...args)`传入参数，调用了person的greet，正因为是person的greet，所以这时候上下文是person的上下文，所以是context，这就完成了this的改变
以上函数使用：
```js
// 一个简单的函数，用来打印 greet 信息
function greet(name, age) {
  console.log(`Hello, my name is ${this.name}, I am ${age} years old, and I know ${name}!`);
}

// 这个对象将成为函数执行时的上下文
const person = {
  name: 'Tom',
};

// 使用 myCall 来执行 greet，并改变 this 的指向为 `person`
greet.myCall(person, 'JavaScript', 25);
```
