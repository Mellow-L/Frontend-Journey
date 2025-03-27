数据绑定：
**v-model:** 双向绑定 `v-model="isLoading"`绑定加载状态
**v-bind:** 单向绑定，简写为：号 
- `:has-more="hasMore"` 完整写法是 `v-bind:has-more="hasMore"`
	- `v-bind：`将js中 `hasMore` 绑定到 `has-more` 属性上
- `v-bind="item"` 将item对象所有属性作为props传递给子组件，子组件用defineProps接收 [[父子组件传参]]
	- 
	- 相当于 `:id="item.id", :title="item.title", :content="item.content"` 具名传入到具体属性。


事件绑定：
**v-on:** 绑定事件，@是v-on的简写形式，可以是子组件emit的事件[[父子组件传参]]，`v-on:click="handleClick"`简写为`@click="handleClick"`

**v-for** 根据 **:key=** 绑定的值显示列表类数据。

**v-if v-else:** 可用于设置按条件的UI渲染