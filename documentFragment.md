DocumentFragments 是DOM节点。它们不是主DOM树的一部分。  
通常的用例是创建文档片段，将元素附加到文档片段，然后将文档片段附加到DOM树。  
在DOM树中，文档片段被其所有的子元素所代替。  
因为文档片段存在于内存中，并不在DOM树中，所以将子元素插入到文档片段时不会引起页面回流(reflow)(对元素位置和几何上的计算)。  
因此，使用文档片段document fragments 通常会起到优化性能的作用(better performance)。  
```js
var index = 0,
  ul = document.querySelector("ul"),
  documentFragment = document.createDocumentFragment(); //创建DocumentFragments
for(var i = 0; i < 10; i++) {
  var li = document.createElement("li")
  li.innerHTML = ++index;
  documentFragment.appendChild(li); //在DocumentFragments中追加元素, 此时的DocumentFragments只存在于内存中
}
ul.appendChild(documentFragment); // 把DocumentFragments追加到文档中, 一次性渲染
```
