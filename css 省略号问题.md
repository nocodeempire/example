css 省略号问题

iOS 不支持整块超长溢出打点省略

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

