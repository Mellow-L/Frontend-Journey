
JavaScript 的事件循环是一种执行模型，它允许 JavaScript 引擎在执行代码时处理异步事件。

任务分为
- 同步任务
- 异步任务 分为
	- 异步宏任务 macrotask，jobs
		- 由宿主发起，比如Node.js，浏览器。
		- 宿主指js的运行环境
		- setTimeout, setInterval, UI事件, message相关
	- 异步微任务 microtask，task
		- 由JS引擎发起。
		- JS引擎是执行JS的核心，比如V8（Chrome 和 Node.js 使用）、SpiderMonkey（Firefox 使用）。负责解析和执行 JavaScript 代码。
		- e.g：promise的.then方法中处理内容为微任务，promise主体是同步任务
		- 微任务队列先于宏任务队列被处理。
		- 

任务处理顺序：
**同步任务-》异步微任务-》异步宏任务**

判断练习：
1. ![[fe340d1d6aae3788996fcd25e16a400.jpg]]
2. ![[c12bfec9b02c66e6ad21316348bc010.jpg]]
3. ![[10c4f0514db245b5a3ba315bcd25fec.jpg]]
JS事件循环 Event-Loop
- JS的异步任务处理