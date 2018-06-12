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
