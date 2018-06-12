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
#### 防抖debounce
````js
// 基础班
var timeout;  //全局变量
function handle() {
  console.log("触发事件了")
}
function debounce() {
  if(timeout) clearTimeout(timeout);
  timeout = setTimeout(handle, 1000)
}
window.addEventListener('scroll', debounce)
````
````js
// 进阶版
function debounce(fn, wait) {
  var timeout = null; //闭包 
  return function() {
    if(timeout !== null) clearTimeout(timeout);
    timeout = setTimeout(fn, wait);
  }
}
function handle() {
  console.log(Math.random());
}
window.addEventListener('scroll', debounce(handle, 1000));
````
