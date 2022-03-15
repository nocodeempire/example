```js
Function.prototype.mybind = function(thisArg, ...args) {
    const fn = this;    // fn mybind的方法调用者(不考虑调用者本身有fn方法)
    thisArg = thisArg !== null && thisArg !== undefined ? Object(thisArg) : window; // 基础类型的话包装下
    return function(...params) {
        thisArg.fn = fn;    // thisArg成为fn的方法调用者
        const argsList = [...args, ...params];
        const result = thisArg.fn(...argsList); // 调用方法后的结果
        delete thisArg.fn;
        return result;
    }
}

```
