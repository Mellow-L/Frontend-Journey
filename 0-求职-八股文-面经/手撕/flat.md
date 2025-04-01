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
`arr.flat(depth)`: 用于扁平化多层嵌套数组，`depth` 指定展开的层级，
- 如 `arr.flat(Infinity)` 可展开所有嵌套。