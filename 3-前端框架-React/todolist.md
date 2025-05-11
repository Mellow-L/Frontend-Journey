### Fragment
- 问题：
	- ![[Pasted image 20250511215132.png]]
- `Fragment`包裹，允许多个根结点
```jsx
import "./styles.css"

export default function App(){
  return (
    <>
      <form className="new-item-form">
        <div className="form-row">
          <label htmlFor="item">New Item</label>
          <input type="text" id="item"></input>
        </div>
      </form>
    </>
  );
}
```
### 钩子
- 让button添加的item响应式地显示到Todo List中：

##### useState钩子
- 设置 `useState`：`const [newItem,setNewItem] = useState("")`

在 `<input>`中设置 `onChange`。其中 `onChange={(e)=>setNewItem(e.target.value)}`。
- 这里 `e`中捕捉到输入值 `e.target.value`

setState方法是异步的。
- 在set后马上log输出，可能还没设置完
- 将setState用回调函数的方式书写
	- 就能每次都在获取到当前todos后进行添加操作
```jsx
setTodos((todos)=>{
      [...todos,{title:newItem,status:false}]
})
//或去掉花括号（均为箭头函数
setTodos((todos)=>[...todos,{title:newItem,status:false}])


//原写法：
setTodos([...todos,{title:newItem,status:false}])
```

##### useEffect钩子
```jsx

```
### 解决警告
警告1：
- ![[Pasted image 20250511205117.png]]
- 解决：
	- 加入`id`，按key=id渲染 即**引入索引标识，防止错误**
		- todo对象定义中：`id:crypto.randomUUID()`
		- ![[Pasted image 20250511205908.png]]
	- ![[Pasted image 20250511205809.png]]


###

```jsx
const toggleTodo = (id,checkStatus)=>{
	setTodos((todos)=>{
	  return todos.map(todo=>{
		if(todo.id === id)return {...todo,status:checkStatus}
		return todo
	  })
	})
}
```

- 对于每一个 `todo` 项：
    - 如果它的 `id` 和传入的 `id` 匹配，就返回一个**新的对象**，该对象复制了原来的所有属性（`...todo`），但把 `status` 改为 `checkStatus`。
    - 否则就保持不变，直接返回原来的 `todo`。

### 短路
在无序列表ul前加：
`{todos.length === 0 && <div>No todos</div>}`
与运算，在判定todos数量后决定是否渲染

### 组件化
- form表单-写todo
- list内容-显示todos

### 插件使用快速创建
| 快捷指令    | 作用                                     |
| ------- | -------------------------------------- |
| `rafce` | 创建一个 `箭头函数 + export` 的 React 组件        |
| `rfce`  | 创建一个 `普通 function + export` 的 React 组件 |
| `imp`   | 快速导入模块                                 |
| `clg`   | 快速生成 `console.log()`                   |

### React组件常见结构

- 钩子 `useEffect`，`useState`等
	- **在函数式组件的顶部实现**
- 方法 函数
- return JSX 里面有HTML也可以有JS

### 组件化的思考

>写组件的意义就在于共用资源，还有程式码的再利用。当一个 APP 变得越来越复杂，如果每个功能都得一再重写实在有够麻烦。虽然我这次的小范例规模不大，可能看不太出来；但你可以想像一下，像是 B 站这样庞大的网站，有好多按钮、影片卡片、还有管理功能，各种元素在不同页面都会用到。工程师不想每次需要写新功能或元件时都得重新来过，所以我们可以预留一些介面，动态地利用资料库的数据在不同页面上显示和重复利用组件。这样前端就能打造一个组件库，而这些组件也通常能保持统一的 UI 设计风格，不至于让一页的程式码变得又臭又长，维护起来超麻烦。另外，一些会在多个页面共用的组件，像是导览列和页尾，也最好独立封装好。