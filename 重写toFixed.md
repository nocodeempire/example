```js
Number.prototype.toFixed = function(digit) {
    const res = this.toLocaleString(undefined, {maximumFractionDigits: digit, useGrouping: false});
    return Number(res).toLocaleString(undefined, {minimumFractionDigits: digit});
}
(111222.115).toFixed(2);  // '111,222.12' (不需要逗号的话 useGrouping: false)
```
