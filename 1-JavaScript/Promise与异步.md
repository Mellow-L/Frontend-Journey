- 异步与同步：Non-Blocking和Blocking，非阻塞和阻塞、
- async 声明异步函数，返回promise对象
- await 等待异步操作结果，实现异步操作同步化
	- ![[Pasted image 20250324164109.png]]
- Promise对象-异步请求返回结果
	- 比如axios返回res实际就是一个promise [[axios请求二次封装]]
	- 包含：
		- data
		- status
		- statusText
		- headers
		- config
	- 表示一个异步操作。
	- 三种状态
		- pending-进行中，待处理（unsettled）
		- resolved-已成功（settled）
		- reject-已失败（settled）
	- 其.then()，resolve()，.catch()，.finally()方法处理结果或错误等，均为异步微任务
	- 主体是同步任务，立即执行

使用实例1：
```js
export async function apiAddItemStar(itemId){
    try {
        let res = await axiosClient.post('/items/put/addstar/'+itemId)
        return Promise.resolve(res?.data)
    } catch (error) {
        alertFail(apiAddItemStar.name,error?.message)
    }
}
```
- 已知这里axiosClient的post请求返回的是一个promise
- await使得异步请求同步化，拿到res后进行下一步
- `Promise.resolve()`直接创建resolved状态的Promise，
	- 等价于`return new Promise((resolve) => resolve(res?.data));`
- 优化：可以省略 Promise.resolve()
```js
return res?.data; 
```
- 因为 async 函数默认返回 Promise，所以直接返回 res?.data 也是 一个已解析的 Promise，不需要 Promise.resolve()。