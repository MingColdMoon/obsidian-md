---
banner: "![[3ee89d866edcb450c164bca52bd110af5d53e248.jpg@942w_564h_progressive_waifu2x_art_noise3_scale.png]]"
banner_y: 0.5
---
# BFC
```ad-quote
title: BFC也叫`块级格式化上下文`，BFC中的元素有一个特性，那就是与外界是隔离的，可以理解为BFC中是一个沙盒环境
```

**触发BFC的条件**
1. html元素下
2. float除了none以外
3. display: inline-block  | flex | table-ceil
4. overflow: visable 以外
5. position: absoulte | fixed

**使用场景（解决了什么问题）**：
1. margin或者padding重叠问题。
```html
.div1 {
	margin-bottom: 10px
}
.div2 {
	margin-top: 20px
}
<div class="div1"></div>
<div class="div2"></div>
```
上面代码中的div2的margin-top会被计算为10px，因为body就是个大的BFC盒子，要想做到div1和div2进行隔离，就需要将两个盒子分别做成BFC
````ad-success
title: 使用BFC后
```html
.div1 {
	overflow: hidden;
	margin-bottom: 10px
}
.div2 {
	overflow: hidden;
	margin-top: 20px
}
<div class="div1"></div>
<div class="div2"></div>
```
````
2. 消除浮动
````ad-success
title: 使用BFC后
```html
<div style="border: 1px solid #000;overflow: hidden">
    <div style="width: 100px;height: 100px;background: #eee;float: left;"></div>
</div>
```
````
3. 浮动元素和BFC元素显示在同一个行
````ad-success
```html
<div style="height: 100px;width: 100px;float: left;background: lightblue">我是一个左浮动的元素</div>
<div style="width: 200px; height: 200px;background: #eee;overflow: hidden">我是一个没有设置浮动</div>
```
````