注意，在使用**箭头函数**时，其中的this并不会绑定到该函数上下文，而是继承自外部函数的this。这是箭头函数的独特之处。[[箭头函数的特点]]
##### 在全局环境下
- 浏览器环境中，this指向**全局对象**，即**window对象**
- 在Node.js环境中，this指向**模块对象**，即**global**

##### 在对象方法中
- this指向调用该方法的对象
- 例如如下代码，greet是obj对象的方法，this指向的是此方法的对象，输出this.name就是输出mellow。
```js
const obj = {
	name:'Mellow',
	greet:function(){
		console.log(this.name);
	}
};
obj.greet();
```

##### 在事件处理函数中
- this指向触发事件的DOM元素
- 如下代码中，this指向按钮元素
```js
const button = document.querySelector('button')
button.addEventListener('click',function(){
	console.log(this);
});
```

