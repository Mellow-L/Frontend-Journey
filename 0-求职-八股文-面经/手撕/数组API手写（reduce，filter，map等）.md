统一解释：
- [[数组相关]]
- 在Array.prototype.xxx中，**this**指的就是调用此方法的数组本身。
- 每个普通函数中，argument对象自动创建。包含了调用该函数时传递的所有参数。
- 在Array.prototype.xxx中，argument指的是这个数组调用xxx方法传入的参数。
	- 比如在myReduce实现中，argument<2的判定，基于reduce传入参数数量，是在对未提供初值的情况进行处理。
- callback
### map
```js
Array.prototype.myMap = function(callback, thisArg) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    // 处理稀疏数组：只有存在的索引才调用 callback
    if (i in this) {
      result.push(callback.call(thisArg, this[i], i, this));
    }
  }
  return result;
};
```
### reduce
```js
Array.prototype.myReduce = function(callback, initialValue) {
  let accumulator = initialValue;
  let i = 0;
  // 如果没有传入初始值，从第一个存在的元素开始作为初始值
  if (arguments.length < 2) {
    while (i < this.length && !(i in this)) {
      i++;
    }
    if (i >= this.length) {
      throw new TypeError('Reduce of empty array with no initial value');
    }
    accumulator = this[i++];
  }
  for (; i < this.length; i++) {
    if (i in this) {
      accumulator = callback(accumulator, this[i], i, this);
    }
  }
  return accumulator;
};
```

### filter
```js
Array.prototype.myFilter = function(callback, thisArg) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback.call(thisArg, this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};
```

### flat
[[数组扁平化 flat]]