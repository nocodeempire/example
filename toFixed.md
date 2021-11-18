```js
Number.prototype.toFixed = function(precision) {
  if(!precision || typeof precision !== "number") return this.toString();
  const weishu = Math.pow(10, precision);
  const res = Math.round(this * weishu) / weishu;
  const numStr = res.toString();
  let [zhengshu, xiaoshu] = numStr.split(".");
  xiaoshu = (xiaoshu || "").padEnd(precision, "0");
  return `${zhengshu}.${xiaoshu}`
}
```
