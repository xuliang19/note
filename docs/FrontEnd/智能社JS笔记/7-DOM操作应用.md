#### 1. 创建，插入和删除元素

##### 1.1 创建DOM元素

创建的元素和HTML本身的元素一样，没有任何区别

- `creatElement(标签名)`创建一个节点
- `appendChild(节点)`追加一个节点（**先将元素从原有父级上删除，再添加到新的父级**）

##### 1.2 插入元素

`insertBefore(节点, 原有节点)`在已有的元素前插入（父级上的方法）

```html
<input type="text" id="content">
<button id="btn">发布</button>
<ul id="ul"></ul>
```

```js
//实现发布效果,后发布的在前面
var oBtn = document.getElementById('btn');
var oContent = document.getElementById('content');
var oUl = document.getElementById('ul');
oBtn.onclick = function() {
    var oLi = document.createElement('li');
    oLi.innerHTML = oContent.value;
    oUl.insertBefore(oLi, oUl.children[0]);
};
```

##### 1.3 删除DOM元素

`removeChild(节点)`删除一个节点

```html
<ul>
    <li><a href="javascript:;">Delet node</a></li>
    <li><a href="javascript:;">Delet node</a></li>
    <li><a href="javascript:;">Delet node</a></li>
    <li><a href="javascript:;">Delet node</a></li>
</ul>
```

```js
var oUl = document.getElementsByTagName('ul')[0];
var aA = document.getElementsByTagName('a');
//每个a绑定click事件
for (var i = 0; i < aA.length; i++) {
    aA[i].onclick = function() {
        oUl.removeChild(this.parentNode);
    }
}
```

#### 2. 文档碎片（DocumentFragment）

每添加一个次节点，就要重新渲染一次；若有多个节点则会影响性能，可将多个节点先添加到`DocumentFragment`里，再一次性追加到元素

理论上可以提高DOM操作性能（对于一些老的浏览器有效，例如IE8），对现代浏览器性能几乎没有提升（用的很少，知道就行）

```js
var oUl = document.getElementById('ul');
var oFrag = document.createDocumentFragment();
for (var i = 0; i < 10000; i++) {
	var oLi = document.createElement('li');
	oFrag.appendChild(oLi);
}
oUl.appendChild(oFrag)
```

