- 用于**改变函数上下文**，即改变其**this的指向**，传入参数。并直接调用一个函数。
- 类似：call，bind [[call方法]]
	- call：接收一系列单独参数（区别于apply，apply接受一个数组）`func.call(thisArg, arg1, arg2, ...);`
	- bind：绑定指定的this和参数，返回新函数。（区别于apply，不返回而是立即调用）
- 用法
	- `func.apply(thisArg,[argsArray])`
	- 参数中，前者为this指向
	- 后者为执行时需要传递的参数列表
- **实例1：理解apply**
```js
function greet(message) {
  console.log(`${message}, ${this.name}!`);
}
const person = {
  name: 'Alice'
};
greet.apply(person, ['Hello']); // 输出: Hello, Alice!
```
- 以上代码解释：
	- apply修改greet的this指向为person对象，传递参数‘hello’。
	- apply同时调用greet函数，输出参数和this.name。
- 实例2：借用函数
```js
const numbers = [5, 2, 8, 1];

// 借用 Math.max
const maxNum = Math.max.apply(null, numbers);
console.log(maxNum); // 输出: 8
```
- 解释：
	- 这里this指向为空，之传入参数numbers数组，相当于调用`Math.max(5,2,8,1)`，即数组numbers借用Math.max方法