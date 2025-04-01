```js
function myPromiseAll(promises) {
  return new Promise((resolve, reject) => {
    const results = [];
    let count = 0;
    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(res => {
          results[index] = res;
          count++;
          if (count === promises.length) resolve(results);
        })
        .catch(err => reject(err));
    });
  });
}

// 测试用例
const p1 = Promise.resolve(1);
const p2 = new Promise((resolve) => setTimeout(() => resolve(2), 1000));

myPromiseAll([p1, p2]).then(console.log); // [1, 2]
```


Promise.all 和 Promise.allSettled
- 都是用来处理多个Promise实例，返回一个新的Promise的方法
- 处理方式和返回结果不同
对于多个promise实例：
```js
const promise1 = new Promise((resolve, reject) => {
	setTimeout(() => resolve('Promise 1 resolved'), 1000);
});
const promise2 = new Promise((resolve, reject) => {
	setTimeout(() => reject('Promise 2 rejected'), 500);
});
const promise3 = new Promise((resolve, reject) => {
	setTimeout(() => resolve('Promise 3 resolved'), 1500);
});
```

==Promise.all方法==：*场景：只关注所有promise需成功*
- 整个Promise被拒绝
```js
Promise.all([promise1, promise2, promise3])
	.then(results => console.log(results))
	.catch(error => console.error(error));
// 输出: Promise 2 rejected
```

==Promise.allSettled方法==：*场景：获取所有promise结果*
- 等待所有promise完成，返回结果数组  
- 其中结果数组包含status  和  value或reason字段
```js
Promise.allSettled([promise1, promise2, promise3])
.then(results => console.log(results))
.catch(error => console.error(error));
// 输出:
// [
// {status: "fulfilled", value: "Promise 1 resolved"},
// {status: "rejected", reason: "Promise 2 rejected"},
// {status: "fulfilled", value: "Promise 3 resolved"}
// ]
```