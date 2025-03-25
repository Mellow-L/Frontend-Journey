官方文档：[arco-design-vue](https://arco.design/vue/component)
全局申明：在`main.js`中：
```js
import ArcoVue from "@arco-design/web-vue";
app.use(ArcoVue)
```

以`MyCard.vue`子组件与其`AllArticles.vue`父组件为例。
- ![[1c84391d6ca5a70c911ddd3e7f8429b.jpg]]
（父组件介绍 [[NutUI使用]]，父子组件传参 [[父子组件传参]]，插槽介绍 [[插槽]]）
- MyCard.vue如下:
```html
<template>
    <a-card :style="{width:'96vw'}">
    --卡片
        <template #actions v-if="props.showMessage">
            <span class="icon-hover" @click="onClickStar">
              <IconThumbUp/>{{props.star}}
            </span>
          <span class="icon-hover" @click="onClickComment">
              <IconMessage/>{{props.comment_count}}
            </span>
          <span class="icon-hover" @click="onClickMore">
              <IconMore/>
            </span>
        </template>
        <template #cover>
          <div class="cover">
            <img class="cover-img"
                 alt="cover"
                 @click="onClickItem"
                 :src="imageSrc"
            />
          </div>
        </template>
        <a-card-meta :title="props.title" :description="props.owner.name">
        -- 使用 Card.Meta 支持更加灵活的内容（封面、头像、标题、描述信息）
          <template #avatar>
            <div class="avatar">
              <a-avatar :size="24" :style="{marginRight:'8px'}">
                <img :alt="avatarAlter" :src="avatarSrc"/>
              </a-avatar>
              <a-typography-text>{{ modify_time }}</a-typography-text>
              -- 排版 Typography，用于展示标题、段落、文本内容。支持不同样式的文本。
            </div>
          </template>
        </a-card-meta>
    </a-card>
</template>
```