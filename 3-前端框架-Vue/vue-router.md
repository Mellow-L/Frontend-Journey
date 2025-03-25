单页应用SPA和多页应用MPA
- SPA：只有一个html文件，通过CSR（客户端渲染）替换页面内容
	- 快速响应
	- 由于JS动态渲染而不利于SEO
- MPA：多个html文件（传统web模式）
	- 适合内容驱动型网站，对SEO要求高

vue中，SPA通常用vue-router实现组件局部切换，提供SSR，很大程度削减了SPA短板

vue-router 页面路由

路由行为常单独封装，比如如下my-router.js:
- routes - 目录，包含path，component，name
- router
	- 由createRouter创建
	- 通常包含history和前面定义的route目录
		- history中的 createWebHashHistory，使路径包含hash字符 “#”，区别于服务器路径（移动端常用） ，不包含则用 createWebHistory

封装后在App.vue中添加< RouterView >组件以显示当前路由：
- 这里用到了作用域插槽 [[插槽]]。
	- 不用插槽，仅靠`<router-view></router-view>`也能够正常路由
	- 这里的插槽使我们能拿到当前匹配的组件Component，然后手动渲染。手动渲染时可以加上keep-alive来缓存组件
- keep-alive作用：
	- 缓存组件，提高性能（比如保留滚动位置、表单数据等）
	- 适用于不需要每次都重新加载的页面，如：
		- 文章列表页面（滚动位置不会丢失）
		- 文章详情页面（不重新请求数据）
-                             
```vue
<template>
  <div>
    <router-view v-slot="{Component}">
      <keep-alive include="MyHome,ShowArticle">
        <component :is="Component" :key="$route.path"></component>
      </keep-alive>
    </router-view>
  </div>
</template>
```
my-router.js
```js
import {createRouter, createWebHashHistory} from "vue-router";
import MyLogin from "@/views/MyLogin.vue";
//import MyProfile from "@/views/MyProfile.vue";
import MyRegister from "@/views/MyRegister.vue";
import MyHome from "@/views/MyHome.vue"
import MySettings from "@/views/MySettings.vue";
import ShowComment from "@/views/ShowComment.vue";
import PostArticle from "@/views/PostArticle.vue";
import ShowArticle from "@/views/ShowArticle.vue";
//import MyProfile from '@/views/TestImageuploader.vue'

const routes = [
    {path:'/',component:MyHome,name:'home'},
    //{path:'/login',component: MyLogin,name:'login'},
    {path:'/login/:back?',component: MyLogin,name:'login'},
    {path:'/register',component: MyRegister,name:'register'},
    {path:'/settings',component: MySettings,name:'settings'},
    {path:'/post-article',component: PostArticle,name:'postArticle'},
    {path:'/show-article/:id',component: ShowArticle,name:'showArticle'},
    {path:'/show-comment/:id',component: ShowComment,name:'showComment',props:true},
    // {path:'/show-article/:id',component: ShowArticle,name:'showArticle',props:true},
];
const router = createRouter({
    history: createWebHashHistory(),
    scrollBehavior(to,from,savedPosition){
        console.log('router','savedPosition',savedPosition);
        if(savedPosition){
            return savedPosition
        }else{
            return {top : 0}
        }
    },
    routes,
})
export default router
export function gotoLogin(goWhere = 0){
    router.push({name:'login',params:{back:goWhere}})
}
export function gotoHome(){
    router.push({name:'home'})
}
export function gotoBack(){
    router.back()
}
export function gotoRegister(){
    router.push({name:'register'})
}
export function gotoSettings(){
    router.push({name:'settings'})
}
export function gotoShowComment(itemId){
    router.push({name:'showComment',params:{id:itemId}})
}
export function gotoShowArticle(itemId){
    router.push({name:'showArticle',params:{id:itemId}})
}
export function gotoPostArticle(){
    router.push({name:'postArticle'})
}
export function replaceToShowArticle(itemId){
    router.replace({name:'showArticle',params:{id:itemId}})
}
```