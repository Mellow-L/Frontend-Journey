ES6新增引用类型 可用new操作符实例化
三种状态，不可逆

### 常见的promise
- 传入一个executor（执行器）作为参数（一个回调）
	- ![[Pasted image 20250401195849.png]]
	- executor通常包含两个参数：resolve，reject。用于控制promise状态
		- resolve，reject：在promise的构造器中会实现这2个内部回调函数。改变promise的state，触发相应回调。
	- executor是立即执行的（整体是同步任务）
```js
const myPromise = new Promise((resolve, reject) => {
//这里(resolve,reject)=>{...}就是一个executor
  setTimeout(() => {
    resolve("成功");
  }, 1000); //1000ms后调用resolve，改变promise状态。一个异步宏任务。
});
```
- 若executor为空，则此promise无法正常执行。


- 如下方法直接返回一个resolved（fulfilled）状态的promise
```js
Promise.resolve()
```

### 链式调用
题目：顺序执行以下两个异步任务
```js
function task1() {
  return new Promise(resolve => setTimeout(() => resolve(1), 1000));
}
function task2(data) {
  return new Promise(resolve => setTimeout(() => resolve(data + 2)), 500);
}
```
answer：
```js
task1().then(res => task2(res))
	.then(finalRes =>console.log(finalRes))
	.catch(err => console.log(err))
```
- 详细解释
	- task1内部立即执行一个executor，内部异步执行（1000 ms后得到1）
	- 执行完毕后触发then，调用task2处理task1的res，即data = 1
	- task2立即执行它的executor，内部异步执行在500 ms后得到1+2=3
	- 执行完毕后触发它的then，执行输出finalRes = 3
	- 整个过程中除了task1和task2内部的promise实例，没有别的promise实例被创建。
	- task1内部返回了一个Promise实例，而这个实例有then方法（根据Promise构造器）用于在状态变更后执行回调。
		- then内`res => task2(res)`等价于`res => {return task2(res)}`
### 错误处理陷阱题
题目：以下代码的输出？
```js
Promise.resolve()
  .then(() => {
    throw new Error('err1');
  })
  .catch(err => console.log('catch1:', err))
  .then(() => console.log('then1'));
```
- 解释：
	- Promise.resolve直接返回一个resolved状态的promise，resolve后进入then。这里直接进入then内部。
	- then内部抛出error，不会改变原来promise的状态，而是返回一个新的reject状态的promise2。**.then总是返回一个新的promise**。err1是它的错误reason
	- promise2 调用了 **. catch也会返回一个新的promise**。处理了promise2的错误，输出catch1: Error: err1。返回一个fulfilled状态的promise3。
	- promise3在被resolved（fulfilled）后调用then方法，输出then1

answer：
- catch1: Error: err1
- then1
### 手写 Promise.all
[[promise.all，allSettled]]
### 手写 Promise.race

### 顺序执行异步任务

### 并发限制实现

### Promise 状态变化模拟

### Promise内部结构
- 包含constructor和then()
	- constructor
	- then()
		- 用于在promise状态变更后执行回调
```js
class MyPromise {
    //NOTE: Promise构造函数
    constructor(executor) {
        this.state = "pending"; // Promise 的初始状态
        this.value = undefined; // 解决或拒绝时的值
        this.reason = undefined; // 拒绝时的原因
        this.onFulfilledCallbacks = []; // 成功后的回调函数队列
        this.onRejectedCallbacks = []; // 失败后的回调函数队列

        //NOTE: Promise.resolve()方法的实现
        const resolve = (value) => {
            if (this.state === "pending") {
                this.state = "fulfilled";
                this.value = value;
                this.onFulfilledCallbacks.forEach((fn) => fn());
            }
        };

        //NOTE: Promise.reject()方法的实现
        const reject = (reason) => {
            if (this.state === "pending") {
                this.state = "rejected";
                this.reason = reason;
                this.onRejectedCallbacks.forEach((fn) => fn());
            }
        };

        try {
            executor(resolve, reject);
        } catch (error) {
            reject(error);
        }
    }
    
    //NOTE: Promise.then()方法的实现
    then(onFulfilled, onRejected) {
      if (this.state === "fulfilled") {
        onFulfilled(this.value);
      } else if (this.state === "rejected") {
        onRejected(this.reason);
      } else {
        this.onFulfilledCallbacks.push(() => onFulfilled(this.value));
        this.onRejectedCallbacks.push(() => onRejected(this.reason));
      }
    }
}

```