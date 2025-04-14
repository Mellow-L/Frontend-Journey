- 前置，讨论响应性
	- 响应式数据有什么用？
		- 核心：自动追踪数据变化，变化时触发视图更新。**实现数据驱动视图**。
			- 无需手动操作DOM
			- 组件间共享数据，并保持同步。
			- 派生出：计算属性 [[computed计算属性]]、watch监听 [[响应式数据与watch监听]]
	- 响应式特性是vue的核心特性。通过 `reactive`、`ref` 等Vue的API 在 JavaScript 数据之上添加了一层代理层，使其具备依赖追踪和派发更新的能力。（原生JS并不具备）
	- React
		- react中，state和hooks提供了类似功能。

响应式的实现--响应式原理：
- vue3使用**Proxy**（from ES6）实现响应式
- vue2使用Object.defineProperty（from ES5）实现响应式。

（vue3使用Proxy替代了Object.defineProperty。）

Object.defineProperty vs Proxy