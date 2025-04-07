new关键字完成的事：（结合实例分析）
1. 创建新对象
2. 将其_proto_属性指向构造函数的prototype（其原型对象）[[原型链]]
3. 执行构造函数内部逻辑，将this绑定到新对象
4. 如果构造函数没有返回一个对象，则返回新对象。否则返回构造函数返回的对象
**手写new**
```js
function myNew(constructor,...args){
	const obj = {}
	obj.__proto__ = constructor.prototype
	const result = constructor.apply(obj,args)
	return result instanceof Object ? result : obj
}
```
- 注意！ proto属性前后是两个短横 _ ！！
- constructor.apply(obj,args)
	- apply方法 [[apply方法]]
	- 将constructor构造函数的this指向obj对象，并传入args参数，立即调用constuctor，得到返回result。
- result instanceof Object ? result : obj
	- 判断result instanceof Object
		- result是否是一个对象？
			- 是就返回result
			- 不是就返回obj
	- 复现逻辑：“如果构造函数没有返回一个对象，则返回新对象。否则返回构造函数返回的对象”
- 应用myNew的变种
- 变种1
	- 以下代码，实例化myCar，调用myNew，传入构造函数Car，传入参数'toyota'
	- 内部创建obj，使其_proto_指向Car的prototype，调用Car（apply的参数分别为新创的obj和传入参数'toyota'）
		- 由于Car构造函数绑定了传入参数
```js
function Car(brand) {
  this.brand = brand;
}
const myCar = myNew(Car, 'Toyota');
console.log(myCar.brand); // 'Toyota'
```
- 变种2
	- 构造函数返回原始值，new将忽略返回值，直接返回整个新创建的对象。
```js
function Car(brand) {
  this.brand = brand;
  return "Hello"; // 返回原始类型值
}
const myCar = myNew(Car, 'Toyota');
console.log(myCar); // Car { brand: 'Toyota' }
```
