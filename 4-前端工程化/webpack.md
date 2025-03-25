webpack：前端资源模块打包工具

webpack使用：
1. 安装webpack
```shell
npm i -D webpack webpack-cli
```
2. 编辑配置文件
- 在**webpack.config.js**（webpack配置文件）中
```shell
module.exports = {
	entry:'main.js' -- 入口文件
	output:{ -- webpack输出位置
		filename:"[name].js", -- 打包后输出的文件
		path:path.join(__dirname,"./dist"), -- 输出文件的位置
	},
	module:{
		rules:[{
			test: --满足条件，比如/\.css$/,
			use: --使用的loader，比如['style-loader','css-loader']
		}]
	}，
	plugins
	mode
	watch
	devServer
	devtool
}
```
- 自入口文件（entry申明）启动编译流程，根据“require”“import”等语句找到依赖资源，资源解析后合并打包
- **Loader**：将非JS资源转换为JS资源。Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。
	- 每个loader任务单一。通过链式调用完成复杂功能。
		- ![[f9d33a0d01f759a70206bda0a588131.jpg]]
		- less-loader 实现less向css的转换
		- css-loader 包装css使之符合js语法
		- style-loader 将css包装进require语句，调用injectStyle等函数将内容注入到页面style标签
	- Babel
		- 将ES6文件经Babel-loader转化为ES5
		- 使用同样是在module中申明babel-loader
3. 执行编译命令
```shell
npx webpack
```

HMR（hot module replacement）模块热替换
- 设置devServer.hot = true

TreeShaking 数摇[[tree-shaking]]