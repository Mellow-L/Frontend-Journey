- 前置，**讨论响应性**
	- 响应式数据有什么用？
		- 核心：自动追踪数据变化，变化时触发视图更新。**实现数据驱动视图**。
			- 无需手动操作DOM
			- 组件间共享数据，并保持同步。
			- 派生出：计算属性 [[computed计算属性]]、watch监听 [[响应式数据与watch监听]]
	- 响应式特性是vue的核心特性。通过 `reactive`、`ref` 等Vue的API 在 JavaScript 数据之上添加了一层代理层，使其具备依赖追踪和派发更新的能力。（原生JS并不具备）
		- reactive和ref底层就是通过Proxy来创建对象
	- React
		- react中，state和hooks提供了类似功能。

响应式的实现--响应式原理：
- vue3使用**Proxy**（from ES6）实现响应式
- vue2使用Object.defineProperty（from ES5）实现响应式。

（vue3使用Proxy替代了Object.defineProperty。）

### Object.defineProperty vs Proxy

#### 共性：
- 均能通过扩展，实现响应式
- ==原理，核心==：
	- 通过 **`get` 拦截属性访问、==收集依赖==（追踪访问）**
		- 依赖收集：自动追踪所有使用响应式数据的位置。（指在访问响应式数据时，记录使用它的地方（如组件或副作用函数），方便之后数据变化时重新运行这些地方。）
	- 通过 **`set` 拦截属性修改、==触发更新==（通知更新）**
		- 触发更新： Vue 将视图绑定到响应式数据，当数据变化时会重新执行“依赖”（如组件的 `render` 函数、computed、watch 回调），从而自动更新页面。
	- vue2和vue3
		- vue2实现：+自定义依赖跟踪系统
		- Vue 3 在 Proxy 的 `get` 中 **手动调用依赖收集逻辑（track）**，在 `set` 中 **手动调用通知逻辑（trigger）**
			- Vue3 是怎么追踪哪些组件要更新的？
				- ==Vue3 中响应式对象被读取时，`get` 会调用 `track()`，记录当前的副作用函数（effect）；当响应式对象修改时，`set` 会调用 `trigger()`，找到相关副作用函数重新执行，从而更新对应组件。==
	- --》 从而实现数据变化驱动视图变化。
	- else：
		- get、set是JS对象的访问器属性。（这涉及JS语法，参见[[对象类型]]）
#### Object.defineProperty (from ES5)
- 介绍：**精确**地添加或修改对象的属性和行为。
	- 只能**拦截**已存在属性。
	- ==Vue 2 内部通过 `Object.defineProperty` + 自定义的“依赖追踪系统”来实现响应式。==
		- 本身并不会自动收集依赖或触发更新，除非你自己手动写这些逻辑
- 使用示例如下代码：
	- 补充理解：
		- *访问对象属性时，内部实际就是调用get方法，改写时，调用其set方法。*
		- 含义：给 `obj` 添加一个叫做 `msg` 的属性，并用我自定义的 `get` 和 `set` 方法来控制它的访问行为。
```js
const obj = {}
Object.defineProperty(obj, 'msg', {
  get() {
    return 'hello'     // 没有依赖收集
  },
  set(val) {
    console.log(val)   // 没有通知更新
  }
})

console.log(obj.msg)  // 输出 'hello'
obj.msg = 'world'     // 打印 '被赋值为 world'
```

#### Proxy (from ES6)
- 介绍：定义一个对象基本操作行为的**拦截器** （读取、写、删除）
	- **监听**整个对象
	- Vue 3 在 Proxy 的 `get` 中 **手动调用依赖收集逻辑（track）**，在 `set` 中 **手动调用通知逻辑（trigger）**。
- 使用：
```js
const proxy = new Proxy(obj, {
  get(target, key) {
    track(target, key) // 依赖收集
    return Reflect.get(target, key)
  },
  set(target, key, value) {
    const result = Reflect.set(target, key, value)
    trigger(target, key) // 触发更新
    return result
  }
})

const obj = { name: 'Mellow' }

const proxy = new Proxy(obj, {  //用Proxy()包裹obj 创建响应式对象
  get(target, key) {
    console.log('读取属性：', key)
    return target[key]
  },
  set(target, key, value) {
    console.log('设置属性：', key, value)
    target[key] = value
    return true
  }
})

proxy.name      // 触发 get，打印：读取属性：name
proxy.age = 22  // 触发 set，打印：设置属性：age 22
```

#### 对比：
| 特性          | `Object.defineProperty`（Vue2） | `Proxy`（Vue3） |
| ----------- | ----------------------------- | ------------- |
| 能否监听新属性添加   | ❌ 否                           | ✅ 是           |
| 能否监听数组变化    | ❌ 需要 hack                     | ✅ 可自动监听       |
| 能否深层监听嵌套对象  | ✅ 需递归处理                       | ✅ 延迟递归（惰性监听）  |
| 性能表现        | 🟡 中等，递归多                     | ✅ 更优，按需监听     |
| API 可读性与维护性 | ❌ 繁琐、侵入性强                     | ✅ 简洁          |


### reactive、ref底层实现
（概念参见：[[响应式数据与watch监听]]）
#### reactive
```js
function reactive(obj) {
  return new Proxy(obj, {
    get(target, key, receiver) {
      track(target, key)  // 依赖收集
      return Reflect.get(target, key, receiver)
    },
    set(target, key, value, receiver) {
      const result = Reflect.set(target, key, value, receiver)
      trigger(target, key) // 通知更新
      return result
    }
  })
}
```
#### ref
```js
function ref(initialValue) {
  const wrapper = {
    value: initialValue
  }
  return reactive(wrapper)
}
```

### 面试常问知识点总结（基于你笔记）：

#### 1. **Vue3 为什么用 Proxy 替代 Object.defineProperty？**

- `Object.defineProperty`
    - 只能监听已有属性，新增属性、删除属性无效。
    - 对于数组操作（如 `push`、`splice`）无法正确追踪，需要 hack。
    - 需要递归处理每一层嵌套对象，性能开销大。
- `Proxy`
    - 可以拦截整个对象，自动监听新增/删除属性。
    - 支持数组等对象原型上的操作。 
    - 具备更好的性能与可维护性，代码简洁。
#### 2. 响应式系统的核心是？（如何实现的
通过 Proxy（或 defineProperty）实现拦截 —— get 时 track 收集依赖，set 时 trigger 触发更新。
在定义响应式数据时，ref和reactive内部定义了proxy对象，proxy对象中设置了get和set方法，get方法在此对象被读取时收集依赖，set方法在此对象被修改时，触发更新get方法中使用过的位置进行同步修改。
在vue2中，响应式通过Object.defineProperty方法实现，用这个方法对某个对象的某个属性显示地设置get和set方法，get方法中收集依赖，set方法中追踪并变化。vue2、vue3两者实际都是通过改变对象的get和set方法实现响应式变化的，所以说有这样的替代关系。
gpt模板：
``在 Vue3 中，响应式系统基于 **ES6 的 Proxy** 实现，其核心原理如下：
- Vue3 中的 `reactive` 和 `ref` 都是通过 Proxy 对象创建的响应式数据。 
- Proxy 拦截对象的 **`get`** 和 **`set`** 操作：  
    - `get`：在访问属性时进行 **依赖收集**（即记录哪些副作用函数使用了这个值）。      
    - `set`：在修改属性时触发 **依赖更新**，从而通知视图或计算属性等地方进行更新。      
相比之下
- Vue2 是通过 `Object.defineProperty` 实现响应式的，它只能对已有的每个属性单独定义 getter 和 setter。  
- 它的局限在于：   
    - 无法监听数组索引、新增属性（如 `obj.a = 123` 不响应）        
    - 需要递归地为对象的每个嵌套属性手动设置 getter/setter。    
因此，Vue3 使用 Proxy 替代了 Vue2 的 `Object.defineProperty`，不仅**性能更好**，而且可以直接监听整个对象的任何操作（包括新增、删除、数组变动等），解决了 Vue2 中的多项局限。``