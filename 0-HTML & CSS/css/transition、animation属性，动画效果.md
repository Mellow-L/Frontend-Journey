transition属性
- 过渡属性：简单的过渡效果
- 需要**事件触发**执行
- 子属性，写成一长条
	- -property：要过渡的属性
	- -duration：持续时间
	- -timing-function：控制速度变化的函数，默认为ease
	- -delay：动画延迟执行的时间
```css
.box{
   width: 100px;
   height:100px;
   transition: width height 2s; -----过渡width，height持续2s
 }
 .box:hover{  -----悬停状态触发
    width:200px;
    height:200px;
 }
```


animation属性
- 动画属性：复杂
- 不需要触发事件
- 子属性，写成一长条
	- -name：@keyframes中定义动画名称
	- -duration：持续时间
	- -timing-function：动画速率变化函数
	- -delay：动画延迟执行时间
	- -iteration-count：执行次数（迭代次数），可以是数字或infinite
	- -direction：动画方向
	- -fill-mode：动画结束后的状态
```css
.box{ 
   animation: name duration timing-function delay iteration-count direction fill-mode play-state;
}
```

相同：
- transition和animation大部分属性是相同的，他们都是随时间改变元素的属性值
区别：
- 事件触发：transition需要触发一个事件才能改变属性，而animation不需要触发任何事件就会随时间改变属性值
- 帧数：transition为两帧，而animation可以是一帧一帧的，跟随自定义动画而言，自定义动画定义了多少帧就执行多少帧
- animation 可以设置很多的属性，比如循环次数，动画结束的状态等等，transition 只能触发一次
- 性能方面：在使用 aniamtion 的时候可以改变很多属性，比如 width、height、position 等等这些改变文档流的属性的时候就会<u>引起页面的回流和重绘</u>，对性能影响比较大，而使用 transition 的时候一般会<u>结合 transform 来进行旋转和缩放等，不会生成新的位图，就不会引起页面的重绘</u>了
