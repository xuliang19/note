#### 1.BOM基础

##### 1.1打开，关闭窗口

`window.close()`在firefox里只能关闭由脚本打开的窗口

```js
oBtn.onclick = function() {
	window.open("http://...");
}
```

`document.write()`先清空内容，然后再写入；每个`window`对应浏览器一个窗口

```js
oBtn.onclick = function() {
	var oWin = window.open();
    // 这里oWin就是新的窗口
	oWin.document.write('<h2>Hello</h2>');
}
```

##### 1.2常用属性

- `window.navigator.userAgent`：浏览器类型、版本等信息
- `window.location`：返回当前网页的地址（还可以赋值）