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
