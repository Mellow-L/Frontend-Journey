- JS中一种基础数据类型（原始数据类型）
- 用于创建**唯一**的标识符
	- 唯一性理解
		-  用法，下面description是一个可选的描述字符串
```js
const symbol1 = Symbol('description');  // 'description' 是可选的描述信息
const symbol2 = Symbol('description');
console.log(symbol1 === symbol2);  // false，两个 Symbol 永远不相等
```

- 常用于**对象的属性名，避免属性名冲突**
```js
const obj = {};
const sym = Symbol('key');// 描述为key
obj[sym] = 'value';
console.log(obj[sym]);  // 'value'
```