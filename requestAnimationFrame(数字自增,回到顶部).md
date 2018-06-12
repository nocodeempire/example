```html
<div></div>
```
```js
// 其实类似setTimeout, 不过这个好处是不需要传时间参数,因为是跟随浏览器的渲染时间自动渲染的.性能佳,不存在卡顿
var divDom = document.querySelector('div');
var distance = 0;
function add() {
  divDom.style.left = ++distance + "px";
  if(distance < 300 ) {
    requestAnimationFrame(add);
  }
}
divDom.addEventListener('click',function(){
  requestAnimationFrame(add);
})
```


## 手动挡的动画

`requestAnimationFrame` 可补充 CSS 样式无法触及的渐变，如渐变 scrollTop 实现页面的平滑滚动。本质上讲：

- 实现 `fps:60`（帧时隙1000ms/60 = 16.7ms）的动画，以保证重绘时间和浏览器刷新频率一致（过频繁浪费算力，过缓慢则不流畅）。所以从它的兼容性定义看，很像是一个 16.7ms 的 setTimeout 函数，需要递归的执行下去。
- 通俗来讲，每 16.7ms，我们修改一次数值，如果把数值的修改反应到DOM中去，就能像幻灯片一样播放起来


```js
// 兼容性的函数定义
function requestAnimationFrame(callback) {
    setTimeout(callback, 16.7)
}
```

动画的实现是通过递归调用，串联起来，形成连续帧的效果

```js
function animate(){
    // ... 跳出条件 + 每一帧的动作
    requestAnimationFrame(animate) // 递归
}
animate() // 执行动画
```

**案例：数字递增动效**

这个效果在钱包类 APP 中常见

![](http://www.imaoda.com/s/img/github/3.gif)

非常简洁的实现：

```js
// DOM中获取元素
let account = document.querySelector('.account')
let number = parseInt(account.innerHTML)
// 在动画中操作元素内容
animate();
function animate() {
    // 递归跳出条件
	if(that.num > 300) return
	// 数字递增显示
	number += 1 
	account.innerHTML = number
	// 递归
	requestAnimationFrame(animate);
}
```

**案例：平滑返回顶端**

scrollTop 属性无法用 CSS3 动画来操控，我们用 requestAnimationFrame 来一帧一帧的实现：

![](http://www.imaoda.com/s/img/github/4.gif)

```js
document.querySelector('.getBack').onclick = function(){
	if (document.documentElement.scrollTop > 0) {
		document.documentElement.scrollTop -= 20 
		requestAnimationFrame (scrollBack)
	}
}
```

## 数字的过渡的插件-Tween.js

在平滑滚回顶端的案例中，每帧的步长是20px，是一个一次线性函数。而 css3 动画默认的 ease 曲线效果不错。[Tween.js](https://github.com/tweenjs/tween.js) 可以辅助我们计算过渡曲线在每一帧的采样值，让 `requestAnimationFrame` 也可以用到丰富的过渡时间曲线（不限于简单粗暴的线性递增）

我们看看用纯 JS 实现动画的案例，了解一下其使用：

![](http://www.imaoda.com/s/img/github/5.gif)

```html
<!--<script src="//cdn.bootcss.com/tween.js/r14/Tween.min.js"></script>-->
<div class='box' style='height:50px;width:50px;background:red'></div>
<script>
let box = document.querySelector('.box')
let coord = { x: 50, y: 50 }
// 创建 tween 对象来操控 coord 对象，coord 将在一段时间内被高频更新
let tween = new TWEEN.Tween(coord)
tween.to({ x: 150, y:150 }, 2000)
tween.onUpdate(function() {
	box.style.height = this.x + 'px'
	box.style.width = this.y + 'px'
})
tween.easing(TWEEN.Easing.Quadratic.Out) // 采用 ease 渐变，可选
tween.start()
// 周期性触发 onUpdate 中的回调（修改元素长宽）
function animate() {
    if(TWEEN.update()) // 触发更新 + 返回值判断跳出递归
	requestAnimationFrame(animate);
}
animate();
</script>
```
Tween 有以下的特点（以上面案例讲解）：

- 在渐变过程中，直接操作到 coord 的地址，其中的数据会被高频更新
- onUpdate 传入的回调函数，会被强制绑定执行者为 coord，执行时 this === coord，通过 this.x 和 coord.x 访问的是同一地址
- tween 成员函数 return this，因此可以串联，如 tween.to().onUpdate().start()
- 执行 start() 之后，coord 对象的数据就开始持续更新了，在对应的时刻调用 TWEEN.update() 才能触发 onUpdate 回调中的动作，比如去调整 DOM

可能有这样的疑问：案例中的效果可以用 transition 很方便实现，何苦写这么多代码？ 没错，尽量用 css 动画，在很多 css 动画无法涉足的领域中可以灵活的用 js 操作数据做补充
