JavaScript 的事件循环（**Event Loop**）是一种执行模型，是==为了解决JS单线程执行中对异步任务处理的问题而设计的一种执行机制。==

任务分为
- 同步任务：主线程上按顺序执行的代码
- 异步任务：由浏览器或Node.js提供的API发起，**结果返回时才回调执行**。 分为
	- 异步宏任务 macrotask，jobs
		- 由宿主发起，比如Node.js，浏览器。
		- 宿主指js的运行环境
		- setTimeout, setInterval, UI事件, message相关
	- 异步微任务 microtask，task
		- 由JS引擎发起。
		- JS引擎是执行JS的核心，比如V8（Chrome 和 Node.js 使用）、SpiderMonkey（Firefox 使用）。负责解析和执行 JavaScript 代码。
		- e.g：promise的.then/catch/finally方法中处理内容为微任务（promise主体是同步任务）等
		- 微任务队列先于宏任务队列被处理。
		- 

任务处理顺序：
- 优先级：**同步任务-》异步微任务-》异步宏任务**
- ==处理顺序描述==
	1. 执行一个宏任务（通常是主线程上的代码）
	2. 在这个宏任务中如果遇到微任务，比如promise.then，先把它加入微任务队列
	3. 在当前宏任务执行完后，立刻清空所有微任务队列，一个接一个执行完。
	4. 再执行下一个宏任务。（不断重复）
判断练习：
1. ![[fe340d1d6aae3788996fcd25e16a400.jpg]]
2. ![[c12bfec9b02c66e6ad21316348bc010.jpg]]
3. ![[10c4f0514db245b5a3ba315bcd25fec.jpg]]
JS事件循环 Event-Loop
- JS的异步任务处理