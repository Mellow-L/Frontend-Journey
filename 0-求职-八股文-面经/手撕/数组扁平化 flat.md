数组扁平化
- 概念：将多维数组转换为一维或较低维度的数组
- 方法：flat(depth)    （array.prototype.flat(depth))
	- depth控制扁平化层级，depth设为 Infinity 可以完全展开数组。
- 比如：
```js
const arr = [1, [2, [3, [4]]]];
console.log(arr.flat(1)); 
// [1, 2, [3, [4]]]
console.log(arr.flat(Infinity)); 
// [1, 2, 3, 4]
```

复现（递归）
- 使用数组reduce方法遍历，展开内容用push存入数组。[[数组相关]]
- 展开一层后depth-1
```js
function flatten(arr, depth = 1) {
  return arr.reduce((acc, item) => {
    if (Array.isArray(item) && depth > 0) {
      acc.push(...flatten(item, depth - 1));
    } else {
      acc.push(item);
    }
    return acc;
  }, []);
}

// 测试用例
console.log(flatten([1, [2, [3]]], 1)); // [1, 2, [3]]
console.log(flatten([1, [2, [3]]], Infinity)); // [1, 2, 3]
```
