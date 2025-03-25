区分JS的宿主和引擎
- 宿主Host指js的运行环境
- JS引擎（Engine）是执行JS的核心，比如V8（Chrome 和 Node.js 使用）、SpiderMonkey（Firefox 使用）。负责解析和执行 JavaScript 代码。


变量提升
- let，const，var，function 四种关键字中
	- var 和 function 的申明将被提到开头，可以被它定义之前的语句访问
	- let 和 const 提前访问将报错

JS执行上下文 execution context分为
- 变量环境 -遇到var创建，对象类型存的是地址
- 词法环境 -遇到function创建
- outer 指向外部环境变量的指针，outer不断向外指形成**作用域链**

执行栈（调用栈）：存放各种执行上下文

JS执行顺序：
1. 扫描到var，function，创建变量环境与词法环境，即**全局上下文入栈**
2. 依次执行，遇到函数，**函数上下文入栈**
3. 函数执行完后，其上下文出栈
