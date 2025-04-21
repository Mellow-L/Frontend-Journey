#### 对比
| 特性       | 作用                    | 使用场景                          | 是否必须显式使用 |
| -------- | --------------------- | ----------------------------- | -------- |
| Fragment | 允许==多个根节点==，减少无意义 DOM | 普通组件返回多个元素，而无根节点              | 否，默认支持   |
| Teleport | 内容==渲染到 DOM 其他节点==    | 模态框、通知、弹出层 需要脱离当前组件结构或渲染到指定节点 | 是        |
| Suspense | ==异步组件加载==状态管理        | 懒加载组件、骨架屏                     | 是        |

#### Fragment

- vue2中，组件必须有一个根节点
	- 错误写法：![[Pasted image 20250419102716.png]]
	- 解决方法：在外层人为地套一个div
		- 但这样导致：1.DOM层级增加 2.样式污染、布局限制
- vue3中，模板渲染默认使用Fragment。允许组件返回多个根节点。解决不必要的DOM层级渲染问题。
	- 上面的错误写法成为正确。
	- （不需要显式声明`<Fragment>`）
	- 本质是一个虚拟容器。**在编译阶段，自动包裹成一个虚拟容器，不会渲染真实DOM元素。**
		- 优势：1.减少不必要的包裹元素 2.更贴合语义化HTML 3.提升组件灵活性和性能

#### Teleport

- 使用场景：模态框、提示框、浮层等组件，希望不被父组件`overflow:hidden`、`z-index`等限制所影响。---直接渲染到body外层或某个节点中。
	- 场景举例：Modal弹窗、Message通知提示、ContextMenu右键菜单、Tooltip悬浮提示等。
	- `overflow:hidden`：控制子元素溢出部分隐藏。此时teleport可以使之脱离父元素裁剪范围，确保其正常显示。
	- `z-index`：控制元素在堆叠上下文中的层级顺序。teleport可使之移出原父组件层级，可置于最顶层。
- 用法：
```vue
<template>
  <teleport to="body"> 
    <div class="modal">弹窗内容</div>
  </teleport>
</template>
```
- 原理
	- （Teleport是vue内置组件）
	- 将其包裹的内容“挂载”到指定的DOM节点。通过to属性控制。但其逻辑和响应式仍保留在当前组件上下文中。
- 问：
	- **Teleport 是否会打乱组件层级？**
		- **不会打乱**，因为 `Teleport` 会将元素渲染到指定位置的 DOM 节点中，但==逻辑仍然保持在当前组件==中。也就是说，**组件的响应式数据、生命周期和事件处理等**依然会在原有组件上下文中执行，**只是视图被“传送”到了 DOM 树的其他地方**。
	- **Teleport 与 appendChild(document.body)的区别**
		- 后者为原生DOM操作。将元素直接添加到body下。但是不保留vue组件的响应式、生命周期管理。
		- 而Teleport不仅能添加到body，也能保留组件响应式和生命周期。


#### Suspense

- 使用场景：延迟加载异步组件（如图表、编辑器等）
- 优点：避免异步加载中的空白页面，优化用户体验。
- 用法：
	- suspense包裹异步组件。组件必须由defineAsyncComponent定义。
	- 如下在#default和#fallback插槽中分别加入异步组件和加载占位。
	- 组件加载时显示fallback中的内容。加载完毕显示default中的内容。
```vue
<template>
  <suspense>
    <template #default>
      <AsyncChart />
    </template>
    <template #fallback>
      <div>加载中...</div>
    </template>
  </suspense>
</template>

<script>
import { defineAsyncComponent } from 'vue'

const AsyncChart = defineAsyncComponent({
  loader: () => import('./MyChart.vue'),
  timeout: 3000,
  delay: 200,
  errorComponent: ErrorComp,
  loadingComponent: LoadingComp,
})
//或者：简易版
const AsyncChart = defineAsyncComponent(() =>
  import('./MyChart.vue')
)
</script>
```
