方法一
```html
<body>
　　<div class="content"></div>
　　<div class="footer"></div>
</body>
```
```css
body { 
    display: flex; 
    flex-flow: column; 
    min-height: 100vh;
 }
 .content {
    flex: 1; 
}
.footer{
    flex: 0;      
}
```
方法二
```html
<body>
    <div class="wrapper clearfix">
        <div class="content"></div>
    </div>　
　　<div class="footer"></div>
</body>
```
```css
html, body, .wrapper {
     height: 100%;
}
body > .wrapper {
     height: auto; min-height: 100%;
}
.content {
    padding-bottom: 150px; /* 必须使用和footer相同的高度 */
}  
.footer {
    position: relative;
    margin-top: -150px; /* footer高度的负值 */
    height: 150px;
    clear:both;
}
.clearfix:after {
     content: "";
     display: table;
     clear: both;
}   
```
