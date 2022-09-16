```js
Number.prototype.toFixed = function(digit = 0) {
    const res = this.toLocaleString(undefined, {maximumFractionDigits: digit, useGrouping: false});
    return Number(res).toLocaleString(undefined, {minimumFractionDigits: digit});
}
(111222.135).toFixed(2);  // '111,222.14' (不需要逗号的话 useGrouping: false)
```
