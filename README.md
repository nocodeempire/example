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
函数防抖（debounce）：当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时。
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
#### 节流throttle
函数节流（throttle）：当持续触发事件时，保证一定时间段内只调用一次事件处理函数。(节流通俗解释就比如我们水龙头放水，阀门一打开，水哗哗的往下流，秉着勤俭节约的优良传统美德，我们要把水龙头关小点，最好是如我们心意按照一定规律在某个时间间隔内一滴一滴的往下滴。)  
函数节流主要有两种实现方法：时间戳和定时器。  
````js
//节流基础班
function throttle(func,delay) {
  var starttime = Date.now();
  return function() {
    var endtime = Date.now();
    if(endtime-starttime>delay){
      func();
      starttime = Date.now();
    }
  }
}
function handle() {
  console.log("节流函数")
}
document.addEventListener("scroll", throttle(handle,1000))
````
````js
//节流throttle代码（时间戳）：
var throttle = function (func, delay) {
  var prev = Date.now();
  return function () {
    var context = this;
    var args = arguments;
    var now = Date.now();
    if (now - prev >= delay) {
      func.apply(context, args);
      prev = Date.now();
    }
  }
}
function handle() {
  console.log(Math.random());
}
window.addEventListener('scroll', throttle(handle, 1000));
````
````js
//节流throttle代码（定时器）：
var throttle = function (func, delay) {
  var timer = null;
  return function () {
    var context = this;
    var args = arguments;
    if (!timer) {
      timer = setTimeout(function () {
        func.apply(context, args);
        timer = null;
      }, delay);
    }
  }
}
function handle() {
  console.log(Math.random());
}
window.addEventListener('scroll', throttle(handle, 1000));
````
````js
//节流throttle代码（时间戳+定时器）：
var throttle = function (func, delay) {
  var timer = null;
   var startTime = Date.now();
   return function () {
      var curTime = Date.now();
      var remaining = delay - (curTime - startTime);
      var context = this;
      var args = arguments;
      clearTimeout(timer);
      if (remaining <= 0) {
        func.apply(context, args);
        startTime = Date.now();
      } else {
        timer = setTimeout(func, remaining);
      }
    }
}
function handle() {
  console.log(Math.random());
}
window.addEventListener('scroll', throttle(handle, 1000));
````













































