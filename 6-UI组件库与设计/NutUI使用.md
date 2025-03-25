
组件使用指南：[NutUI - 移动端 Vue 组件库](https://nutui.jd.com/h5/vue/4x/#/zh-CN/component/button)

全局申明：在`main.js`中：
```js
import NutUI from "@nutui/nutui"
app.use(NutUI)
```

以文章列表页`AllArticles.vue`为例：
- **`nut-empty`**
- **`nut-ellipsis`**
- **`nut-button`**
- **`nut-infinite-loading`**
- **`nut-space`**
- **`nut-pull-refresh`**
```html
<template>
    <nut-empty v-if="error" image="error" description="Error">
     -- 用于显示错误状态时的占位内容，提供图片，错误描述，用v-if绑定状态。
        <nut-cell>
        -- 单元格，即列表项，可组成列表，可定义插槽
            <nut-ellipsis :content="error"
                direction="end"
                expand-text="展开"
                rows="3"
                class="my-ellipsis"
                collapse-text="收起">
                -- 用于对绑定的文本进行省略处理
                -- direction控制省略部分的位置
            </nut-ellipsis>
        </nut-cell>
        <nut-button type="primary" style="margin-top: 5px;"
            :loading="isLoading"
            @click="refreshFun">Refresh</nut-button>
    </nut-empty>
    <div v-if="allList" class="center">
        <nut-infinite-loading v-model="isLoading"
                :has-more="hasMore"
                @scroll-change="scrollChange"
                @load-more="loadMore">
            -- InfiniteLoading 滚动加载，列表滚动到底部自动加载更多数据。
            -- v-model="isLoading"绑定加载状态
            -- :has-more="hasMore"完整写法是 v-bind:has-more="hasMore"。 
            -- v-bind：将js中hasMore绑定到has-more属性上
            -- @是v-on的简写形式，用于绑定事件监听器，v-on:click="handleClick"简写为@click="handleClick"
            -- 滚动加载UI绑定两个函数
            <nut-space v-if="!counterEnableRef"
                direction="vertical"
                align="center" fill>
                -- 设置元素间距
                <nut-pull-refresh v-if="!counterEnableRef"
                    v-model="isLoading" @refresh="refreshFun">
                    -- 用于提供下拉刷新的交互操作。
                    <MyCard v-for="item in list" :key="item.id" v-bind="item"
                        @onClickStar="(id)=>clickStar(id)"
                        @onClickComment="gotoShowComment(item.id)"></MyCard>
                        -- 使用了自定义组件Mycard，v-bind="item"将item对象所有属性一次性绑定到此组件，而item.id绑定到key属性，v-for通过key属性显示数组list中的item数据
                </nut-pull-refresh>
            </nut-space>
            <nut-space v-else
                direction="vertical" align="center" fill>
                <MyCard v-for="item in list" :key="item.id" v-bind="item"
                    @onClickStar="(id)=>clickStar(id)"
                    @onClickComment="gotoShowComment(item.id)"></MyCard>
            </nut-space>
        </nut-infinite-loading>   
    </div>
</template>
```

- **`nut-uploader`**
	- url：服务器接口地址 （实例代码单向绑定父组件传入的url）
	- name：字段名
	- headers：上传头信息
	- @delete，@success：v-on简写，绑定了两个事件
	- capture：启用设备相机
```vue
<template>

	<nut-uploader 
		:url="props.url" 
		name="file"
	    v-if="props.needCompression" 
	    :headers="headToken"
	    v-model:file-list="defaultFileList"
	    @success="onImgSuccess"
	    @delete="deleteImageByFileName"
	    list-type="list"
	    :before-upload="beforeUpload">
	    <nut-button type="info" size="small"
		    v-if="defaultFileList.length === 0">上传图片
	    </nut-button>
	</nut-uploader>
</template>
<script setup lang="js">

	const onImgSuccess = ((response)=>{
	  //console.log(responseText)
	  showSuccess(onImgSuccess.name, response)
	  let resObj;
	  try {
	    resObj = JSON.parse(response.responseText);
	    console.log("解析后的resObj.src:", resObj.src);
	  } catch (error) {
	    showFail("JSON解析失败", error);
	    return;
	  }
	  imgSrc.value = resObj.src;
	  emit('onSuccess',resObj.src)
	})
</script>
```
- **`nut-row, nut-col`**
- **`nut-tabbar`** 标签栏组件，用于实现底部导航栏
	- placeholder 为此组件所在位置预留空间
```vue
<template>
    <keep-alive>
        <component :is="currentComponent"></component>
    </keep-alive>
    <nut-tabbar v-model="activeTab" @tab-switch="tabSwitch"
        unactive-color="#505050"
        bottom safe-area-inset-bottom placeholder>
        <nut-tabbar-item v-for="item in List"
            :key="item.name"
            :tab-title="item.title"
            :icon="item.icon"> 
        </nut-tabbar-item>
    </nut-tabbar>
</template>
```