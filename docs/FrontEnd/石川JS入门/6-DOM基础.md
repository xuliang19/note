#### 1. DOM节点

- `childNodes`获取所有子节点，`nodeType`获取节点类型（可以直接使用`children`代替）

  ```html
  <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
  </ul>
  ```

  ```js
  //使用childNodes获取上述节点的数量为9;4个元素节点,5个空白文本节点
  var oUl = document.getElementsByTagName('ul')[0];
  //使用nodeType剔除空白文本节点
  for (var i = 0; i < oUl.childNodes.length; i++) {
      if (oUl.childNodes[i].nodeType === 1) {
          //some code  
      }
  }
  ```

  上述其实可以直接使用`element`上的`chidren`属性来获取四个`<li>`

- `parentNode`获取父节点，例子

  ```html
  <ul>
      <li><a href="javascript:;">隐藏</a>123</li>
      <li><a href="javascript:;">隐藏</a>123</li>
      <li><a href="javascript:;">隐藏</a>123</li>
      <li><a href="javascript:;">隐藏</a>123</li>
  </ul>
  ```

  ```js
  //点击隐藏时,隐藏<li>元素
  var aA = document.getElementsByTagName('a');
  for (var i = 0; i < aA.length; i++) {
      aA[i].onclick = function() {
          this.parentNode.style.display = 'none';
      }
  }
  ```

- `offsetParent`获取最近的具有定位属性的父元素

#### 2. DOM节点（2）

- 首尾节点

  - `firstChild`, `firstElementChild`
  - `lastChild`, `lastElementChild`

  解决兼容问题基本都这个套路，判断有无，再使用对应属性

  ```js
  //解决兼容问题
  if (oUl.firstElementChild) {
      oUl.firstElementChild.style.background = 'red';
  }
  else {
      oUl.firstChild.style.background = 'red';
  }
  ```

- 兄弟节点

  - `nextSibling`, `nextElementSibling`
  - `previousSibling`, `previousElementSibling`

#### 3. 操作元素属性

- 第一种：`oDiv.style.display='block'`
- 第二种：`oDiv.style['display']='block'`
- 第三种：DOM方式

##### DOM方式操作元素属性

- 获取：`getAttribute(名称)`
- 设置：`setAttribute(名称, 值)`
- 删除：`removeAttribute(名称)`

#### 4. 元素灵活查找

用`className`选择元素

```js
//封装一个通过类名选择元素的函数
function getByClass(oParent, sClass) {
    var aResult = [];
    var aEle = oParent.getElementsByTagName('*');
    for (var i = 0; i < aEle.length; i++) {
        if (aEle[i].className === sClass) {
            aResult.push(aEle[i]);
        }
    }
    return aResult;
}
```

?> 需要返回多个值的时候,放数组里再`return`

