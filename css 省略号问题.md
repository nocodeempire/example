css 省略号问题

iOS 不支持整块超长溢出打点省略
我们将多行省略的代码替换单行省略，只是行数 -webkit-line-clamp: 2 改成一行即可 -webkit-line-clamp: 1。
值得注意的是，在使用 -webkit-line-clamp 的方案的时候，一定要配合 white-space: normal 允许换行，而不是不换行。这一点，非常重要。

```css
.person-card__desc {
    width: 200px;
    white-space: normal;
    overflow : hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
}
.person-card__desc span {
    display: inline-block;
}
```

