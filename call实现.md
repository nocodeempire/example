```js
Function.prototype.mycall = function(thisArg, ...args) {
    var fn = this;
    thisArg = thisArg ? Object(thisArg) : window;
    thisArg.fn = this;
    var result = thisArg.fn(...arg);
    delete thisArg.fn;
    return result;
}

```
