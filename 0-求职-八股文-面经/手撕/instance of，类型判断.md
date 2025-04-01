
用于**判断某个对象是否是某个构造函数的实例**
- 原理是沿 `__proto__` 原型链向上查找 `prototype` 是否匹配。
- 方式：沿着对象的 `__proto__` 原型链查找，直到找到 `constructor.prototype`，否则返回 `false`。（检测构造函数的prototype 属性是否出现在某个实例对象的原型链上。）[[原型链]]
### 手写代码
```js
 let myInstanceof = (target,origin) => {
     while(target) {
         if(target.__proto__===origin.prototype) {
            return true
         }
         target = target.__proto__
     }
     return false
 }
 let a = [1,2,3]
 console.log(myInstanceof(a,Array));  // true
 console.log(myInstanceof(a,Object));  // true
```

- 用法：
	- xxx instance of xxxxx，返回值为Boolean值
```js
// 定义构造函数
function C() {}
function D() {}

// 实例化一个对象
var o = new C();

console.log(o instanceof C); // true，因为 C.prototype 在 o 的原型链上
console.log(o instanceof D); // false，因为 D.prototype 不在 o 的原型链上
console.log(o instanceof Object); // true，因为 Object.prototype 在 o 的原型链上

//o的原型链结构
o ---> C.prototype ---> Object.prototype ---> null

```
- 原理验证：基于上面的代码
	- 实例创建后，它的 `__proto__` 绑定的是创建时的 `Constructor.prototype`，不会随 `prototype` 变化而改变。
	- `o.__proto__ === 旧的 C.prototype`（不等于新的 `{}`）
	- `o instanceof C // false`。
```js
// 修改构造函数 C 的原型为一个空对象
C.prototype = {}

var o2 = new C()
console.log(o2 instanceof C); // true，因为 C.prototype 在 o2 的原型链上
console.log(o instanceof C); // false，因为 C.prototype 指向了一个空对象，这个空对象不在 o 的原型链上
```
- 继承（基于上面代码）
```js
// 继承
D.prototype = new C();
var o3 = new D();

console.log(o3 instanceof D); // true，因为 D.prototype 在 o3 的原型链上
console.log(o3 instanceof C); // true，因为 C.prototype 现在在 o3 的原型链上
```

其他类型判断：

**typeof**
- 用于基本数据类型
- 不能区分引用类型数据（对象，数组，正则等）
```js
console.log(typeof 123); // "number"
console.log(typeof "hello"); // "string"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof Symbol("id")); // "symbol"
console.log(typeof BigInt(9007199254740991)); // "bigint"
console.log(typeof function(){}); // "function"

console.log(typeof []); // "object" ❌（不能区分数组）
console.log(typeof {}); // "object"
console.log(typeof null); // "object" ❌（历史遗留问题）
```

**Object . prototype . toString . call(xxxxx)**
- 适用于所有类型
```js
console.log(Object.prototype.toString.call(null)); // "[object Null]"
console.log(Object.prototype.toString.call([])); // "[object Array]"
console.log(Object.prototype.toString.call({})); // "[object Object]"
console.log(Object.prototype.toString.call(/\d+/)); // "[object RegExp]"
console.log(Object.prototype.toString.call(new Date())); // "[object Date]"
console.log(Object.prototype.toString.call(undefined)); // "[object Undefined]"
console.log(Object.prototype.toString.call(function(){})); // "[object Function]"
console.log(Object.prototype.toString.call(new Map())); // "[object Map]"
console.log(Object.prototype.toString.call(new Set())); // "[object Set]"
```