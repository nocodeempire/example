# 记录一些例子
#### requestAnimationFrame
````hrml
<div></div>
````
````js
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
````
