位于< head >内，定义了html文档的元数据
- 不显示在页面上，对于搜索引擎可读。
- 举例
	- 文档字符集、页面描述、关键词、文档作者、视口设置
```html
<meta charset="UTF-8">
<meta name="description" content="免费的 Web 教程">
<meta name="keywords" content="HTML, CSS, JavaScript">
<meta name="author" content="John Doe">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- 属性
	- charset：声明文档使用的字符编码。
	- content：与nam 或http-equiv属性相关联的值。
	- http-equiv：提供 HTTP 头信息，如页面刷新。
	- name：定义元数据的名称，如 description、keywords、author。