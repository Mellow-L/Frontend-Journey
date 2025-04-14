- URL解析：将URL字符串分解为各组成部分
- URL结构
	- 包含协议、账密、主机名、端口号、路径名、查询参数等。
	- 图示![[FE-notes/0-求职-八股文-面经/手撕/attachments/Pasted image 20250408220455.png]]
- **URL构造函数**：（实际开发中常见）
	- 用 new URL() 创建url。接受一个绝对URL来创建。或接受一个相对URL以及一个对应的绝对URL来创建。
	- 创建的url对象可以访问其各部分属性。比如`console.log(url.href);`，`console.log(url.hostname);`这样。
- 手动解析URL ？？？
	-  正则解析 [[正则表达式]]
```js
function parseURL(url) {
  const regex = /^(https?):\/\/([^\/:]+)(?::(\d+))?(\/[^?#]*)?(\?[^#]*)?(#.*)?$/;
  const matches = url.match(regex);
  return {
    protocol: matches[1],
    hostname: matches[2],
    port: matches[3] || '',
    pathname: matches[4] || '',
    search: matches[5] || '',
    hash: matches[6] || ''
  };
}
```
