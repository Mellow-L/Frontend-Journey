ECMAScript 6（简称ES6）是**JavaScript语言的下一代标准**，自2015年6月正式发布以来，它引入了许多新的语言特性，使得JavaScript编程更加强大和灵活。
ES6的这些新特性极大地提升了JavaScript的编程体验和能力，使得开发者可以编写更加简洁、高效和强大的代码。
以下是ES6中一些显著的新特性：

##### 新的数据类型和变量声明

**Symbol**: ES6引入了一种新的**原始数据类型**Symbol，它代表独一无二的值，主要用于创建对象的私有成员。每个通过Symbol()函数创建的值都是唯一的，即使传递相同的参数也不相等

 `let sym1 = Symbol('key'); let sym2 = Symbol('key'); console.log(sym1 === sym2); // 输出: false`

**let和const**: 这两个关键字用于变量声明，提供了块级作用域（block-scoping）。let允许在块级作用域中声明变量，而**const用于声明常量**，一旦声明，其值就不能改变

 `let a = 2; { let a = 3; console.log(a); // 输出: 3 } console.log(a); // 输出: 2 const b = 1; // b = 2; // 报错: TypeError: Assignment to constant variable.`

##### 解构赋值

数组和对象的解构赋值: ES6允许按照一定模式**从数组和对象中提取值**，对变量进行赋值，这被称为解构

```js
let [x, y, z] = [1, 2, 3]; console.log(x); // 输出: 1
let {name, age} = {name: 'Alice', age: 25}; console.log(name); // 输出: Alice
```

#####  函数新特性

参数默认值: ES6允许**函数参数有默认值**，简化了函数的初始化过程

 `function log(x, y = 'World') { console.log(x, y); } log('Hello'); // 输出: Hello World`

**箭头函数**: 箭头函数提供了一种更简洁的函数写法，并且它不绑定自己的this，this值将是在定义时的上下文
在使用箭头函数时，其中的this并不会绑定到该函数上下文，而是继承自外部函数的this
`const add = (a, b) => a + b; console.log(add(1, 2)); // 输出: 3`

##### 新的对象和方法

**Map和Set**: ES6提供了新的数据结构Map和Set。Map是**键值对**的集合，Set是值的集合，且**集合中的值都是唯一的**

```js
let map = new Map(); map.set('key', 'value'); console.log(map.get('key')); // 输出: value
let set = new Set([1, 2, 3, 2]); console.log(set.size); // 输出: 3
```

对象字面量增强: 对象字面量的属性定义更加简洁，支持设置原型，定义方法，调用父类的方法等

`let obj = { __proto__: theProtoObj, handler, toString() { // 覆盖原型链上的toString方法 return 'd ' + super.toString(); }, };`

##### 异步编程

**Promise**: **ES6引入**了Promise对象，它是**异步编程的一种解决方案**，允许将回调式的异步编程转换为基于Promise的链式调用

`let promise = new Promise(function(resolve, reject) { // 异步操作成功时调用resolve，失败时调用reject });`

**Generator:** 生成器函数可以通过yield关键字暂停函数执行，并在适当的时候恢复，这为异步编程提供了新的解决方案

`function* helloWorldGenerator() { yield 'hello'; yield 'world'; return 'ending'; } var hw = helloWorldGenerator();`

**async/await:** **ES8引入**了async函数，它是**Generator函数的语法糖**。使用async/await可以更方便地编写异步代码

`async function getStockPriceByName(name) { const symbol = await getStockSymbol(name); const stockPrice = await getStockPrice(symbol); return stockPrice; }`


