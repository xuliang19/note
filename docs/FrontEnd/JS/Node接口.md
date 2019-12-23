所有DOM节点对象都继承了Node接口，拥有一些共同的属性和方法。这是DOM操作的基础

#### 1. 属性

|               属性               |                         作用                         |
| :------------------------------: | :--------------------------------------------------: |
|    `Node.prototype.nodeType`     |              返回一个数值，表示节点类型              |
|    `Node.prototype.nodeName`     |                   返回节点大写名称                   |
|    `Node.prototype.nodeValue`    |     返回字符串，表示当前节点本身的文本值，可读写     |
|   `Node.prototype.textContent`   |        返回当前节点和它所有后带节点的文本内容        |
|     `Node.prototype.baseURI`     |       返回字符串，表示当前网页的绝对路径，只读       |
|  `Node.prototype.ownerDocument`  |     返回当前节点所在的顶层文档对象，即`document`     |
|   `Node.prototype.nextSibling`   | 返回紧跟当前节点后的第一个同级节点，没有则返回`null` |
| `Node.prototype.previousSibling` |  返回当前节点前一个紧跟的同级节点，没有则返回`null`  |
|   `Node.prototype.parentNode`    |                 返回当前节点的父节点                 |
|  `Node.prototype.parentElement`  |      返回当前节点的父元素节点，没有则返回`null`      |
|   `Node.prototype.firstChild`    |     返回当前节点的第一个子节点，没有则返回`null`     |
|    `Node.prototype.lastChild`    |    返回当前节点的最后一个子节点，没有则返回`null`    |
|   `Node.prototype.childNodes`    |   返回一个类似数组的对象，包括当前节点和所有子节点   |
|   `Node.prototype.isConnected`   |       返回一个布尔值，表示当前节点是否在文档中       |

##### 1.1 Node.prototype.nodeType

`nodeType`属性返回一个整数值，表示节点的类型。

```js
document.nodeType // 9
```

上面代码中，文档节点的类型值为9。

Node 对象定义了几个常量，对应这些类型值。

```js
document.nodeType === Node.DOCUMENT_NODE // true
```

上面代码中，文档节点的`nodeType`属性等于常量`Node.DOCUMENT_NODE`。

不同节点的`nodeType`属性值和对应的常量如下。

- 文档节点（document）：9，对应常量`Node.DOCUMENT_NODE`
- 元素节点（element）：1，对应常量`Node.ELEMENT_NODE`
- 属性节点（attr）：2，对应常量`Node.ATTRIBUTE_NODE`
- 文本节点（text）：3，对应常量`Node.TEXT_NODE`
- 文档片断节点（DocumentFragment）：11，对应常量`Node.DOCUMENT_FRAGMENT_NODE`
- 文档类型节点（DocumentType）：10，对应常量`Node.DOCUMENT_TYPE_NODE`
- 注释节点（Comment）：8，对应常量`Node.COMMENT_NODE`

确定节点类型时，使用`nodeType`属性是常用方法。

```js
var node = document.documentElement.firstChild;
if (node.nodeType === Node.ELEMENT_NODE) {
  console.log('该节点是元素节点');
}
```

##### 1.2 Node.prototype.nodeName

`nodeName`属性返回节点的名称（大写）

```js
// HTML 代码如下
// <div id="d1">hello world</div>
var div = document.getElementById('d1');
div.nodeName // "DIV"
```

上面代码中，元素节点`<div>`的`nodeName`属性就是大写的标签名`DIV`。

不同节点的`nodeName`属性值如下。

- 文档节点（document）：`#document`
- 元素节点（element）：大写的标签名
- 属性节点（attr）：属性的名称
- 文本节点（text）：`#text`
- 文档片断节点（DocumentFragment）：`#document-fragment`
- 文档类型节点（DocumentType）：文档的类型
- 注释节点（Comment）：`#comment`

##### 1.3 Node.prototype.nodeValue

`nodeValue`属性返回一个字符串，表示当前节点本身的文本值，该属性可读写。

只有文本节点（text）、注释节点（comment）和属性节点（attr）有文本值，因此这三类节点的`nodeValue`可以返回结果，其他类型的节点一律返回`null`。同样的，也只有这三类节点可以设置`nodeValue`属性的值，其他类型的节点设置无效。

```js
// HTML 代码如下
// <div id="d1">hello world</div>
var div = document.getElementById('d1');
div.nodeValue // null
div.firstChild.nodeValue // "hello world"
```

上面代码中，`div`是元素节点，`nodeValue`属性返回`null`。`div.firstChild`是文本节点，所以可以返回文本值。

##### 1.4 Node.prototype.textContent

`textContent`属性返回当前节点和它的所有后代节点的文本内容。

```js
// HTML 代码为
// <div id="divA">This is <span>some</span> text</div>
document.getElementById('divA').textContent
// This is some text
```

`textContent`属性自动忽略当前节点内部的 HTML 标签，返回所有文本内容。

该属性是可读写的，设置该属性的值，会用一个新的文本节点，替换所有原来的子节点。它还有一个好处，就是自动对 HTML 标签转义。这很适合用于用户提供的内容。

```js
document.getElementById('foo').textContent = '<p>GoodBye!</p>';
```

上面代码在插入文本时，会将`<p>`标签解释为文本，而不会当作标签处理。

对于文本节点（text）、注释节点（comment）和属性节点（attr），`textContent`属性的值与`nodeValue`属性相同。对于其他类型的节点，该属性会将每个子节点（不包括注释节点）的内容连接在一起返回。如果一个节点没有子节点，则返回空字符串。

文档节点（document）和文档类型节点（doctype）的`textContent`属性为`null`。如果要读取整个文档的内容，可以使用`document.documentElement.textContent`。

##### 1.5 Node.prototype.baseURI

`baseURI`属性返回一个字符串，表示当前网页的绝对路径。浏览器根据这个属性，计算网页上的相对路径的 URL。该属性为只读。

```js
// 当前网页的网址为
// http://www.example.com/index.html
document.baseURI
// "http://www.example.com/index.html"
```

如果无法读到网页的 URL，`baseURI`属性返回`null`。

该属性的值一般由当前网址的 URL（即`window.location`属性）决定，但是可以使用 HTML 的`<base>`标签，改变该属性的值。

```js
<base href="http://www.example.com/page.html">
```

设置了以后，`baseURI`属性就返回`<base>`标签设置的值。

##### 1.6 Node.prototype.ownerDocument

`Node.ownerDocument`属性返回当前节点所在的顶层文档对象，即`document`对象。

```js
var d = p.ownerDocument;
d === document // true
```

`document`对象本身的`ownerDocument`属性，返回`null`。

##### 1.7 Node.prototype.nextSibling

`Node.nextSibling`属性返回紧跟在当前节点后面的第一个同级节点。如果当前节点后面没有同级节点，则返回`null`。

```js
// HTML 代码如下
// <div id="d1">hello</div><div id="d2">world</div>
var d1 = document.getElementById('d1');
var d2 = document.getElementById('d2');
d1.nextSibling === d2 // true
```

上面代码中，`d1.nextSibling`就是紧跟在`d1`后面的同级节点`d2`。

注意，该属性还包括文本节点和注释节点（`<!-- comment -->`）。因此如果当前节点后面有空格，该属性会返回一个文本节点，内容为空格。

`nextSibling`属性可以用来遍历所有子节点。

```js
var el = document.getElementById('div1').firstChild;
while (el !== null) {
  console.log(el.nodeName);
  el = el.nextSibling;
}
```

上面代码遍历`div1`节点的所有子节点。

##### 1.8 Node.prototype.previousSibling

`previousSibling`属性返回当前节点前面的、距离最近的一个同级节点。如果当前节点前面没有同级节点，则返回`null`。

```js
// HTML 代码如下
// <div id="d1">hello</div><div id="d2">world</div>
var d1 = document.getElementById('d1');
var d2 = document.getElementById('d2');
d2.previousSibling === d1 // true
```

上面代码中，`d2.previousSibling`就是`d2`前面的同级节点`d1`。

注意，该属性还包括文本节点和注释节点。因此如果当前节点前面有空格，该属性会返回一个文本节点，内容为空格。

##### 1.9 Node.prototype.parentNode

`parentNode`属性返回当前节点的父节点。对于一个节点来说，它的父节点只可能是三种类型：元素节点（element）、文档节点（document）和文档片段节点（documentfragment）。

```js
if (node.parentNode) {
  node.parentNode.removeChild(node);
}
```

上面代码中，通过`node.parentNode`属性将`node`节点从文档里面移除。

文档节点（document）和文档片段节点（documentfragment）的父节点都是`null`。另外，对于那些生成后还没插入 DOM 树的节点，父节点也是`null`。

##### 1.10 Node.prototype.parentElement

`parentElement`属性返回当前节点的父元素节点。如果当前节点没有父节点，或者父节点类型不是元素节点，则返回`null`。

```js
if (node.parentElement) {
  node.parentElement.style.color = 'red';
}
```

上面代码中，父元素节点的样式设定了红色。

由于父节点只可能是三种类型：元素节点、文档节点（document）和文档片段节点（documentfragment）。`parentElement`属性相当于把后两种父节点都排除了。

##### 1.11 Node.prototype.firstChild，Node.prototype.lastChild

`firstChild`属性返回当前节点的第一个子节点，如果当前节点没有子节点，则返回`null`。

```js
// HTML 代码如下
// <p id="p1"><span>First span</span></p>
var p1 = document.getElementById('p1');
p1.firstChild.nodeName // "SPAN"
```

上面代码中，`p`元素的第一个子节点是`span`元素。

注意，`firstChild`返回的除了元素节点，还可能是文本节点或注释节点。

```js
// HTML 代码如下
// <p id="p1">
//   <span>First span</span>
//  </p>
var p1 = document.getElementById('p1');
p1.firstChild.nodeName // "#text"
```

上面代码中，`p`元素与`span`元素之间有空白字符，这导致`firstChild`返回的是文本节点。

`lastChild`属性返回当前节点的最后一个子节点，如果当前节点没有子节点，则返回`null`。用法与`firstChild`属性相同。

##### 1.12 Node.prototype.childNodes

`childNodes`属性返回一个类似数组的对象（`NodeList`集合），成员包括当前节点的所有子节点（直接子节点哦）

```js
var children = document.querySelector('ul').childNodes;
```

上面代码中，`children`就是`ul`元素的所有子节点。

使用该属性，可以遍历某个节点的所有子节点。

```js
var div = document.getElementById('div1');
var children = div.childNodes;
for (var i = 0; i < children.length; i++) {
  // ...
}
```

文档节点（document）就有两个子节点：文档类型节点（docType）和 HTML 根元素节点。

```js
var children = document.childNodes;
for (var i = 0; i < children.length; i++) {
  console.log(children[i].nodeType);
}
// 10
// 1
```

上面代码中，文档节点的第一个子节点的类型是10（即文档类型节点），第二个子节点的类型是1（即元素节点）。

注意，除了元素节点，`childNodes`属性的返回值还包括文本节点和注释节点。如果当前节点不包括任何子节点，则返回一个空的`NodeList`集合。由于`NodeList`对象是一个动态集合，一旦子节点发生变化，立刻会反映在返回结果之中。

##### 1.13Node.prototype.isConnected

#### 2. 方法

|                    方法                    |                             作用                             |
| :----------------------------------------: | :----------------------------------------------------------: |
|       `Node.prototype.appendChild()`       | 接受一个节点对象为参数插入当前节点最后子节点后，返回插入的节点 |
|      `Node.prototype.hasChildNodes()`      |             返回布尔值，表示当前节点是否有子节点             |
|        `Node.prototype.cloneNode()`        | 克隆一个节点，接受布尔值为参数表示是否同时克隆子节点，返回值是新克隆出的节点 |
|      `Node.prototype.insertBefore()`       |               将某个节点插入父节点内部指定位置               |
|       `Node.prototype.removeChild()`       | 接受一个子节点为参数，从当前节点中移除该子节点，返回移除的子节点 |
|      `Node.prototype.replaceChild()`       |          将一个新的节点，替换当前节点的某一个子节点          |
|        `Node.prototype.contains()`         |       返回一个布尔值，表示参数节点是否满足三个条件之一       |
| `Node.prototype.compareDocumentPosition()` | 与上述方法一致，返回一个六个比特位额二进制值，表示参数节点和当前节点关系 |
|       `Node.prototype.isEqualNode()`       |             返回一个布尔值，表示两个节点是否相等             |
|       `Node.prototype.isSameNode()`        |         返回一个布尔值，表示两个节点是否为同一个节点         |
|        `Node.prototype.normalize()`        |               清理当前界节点内部的所有文本节点               |
|       `Node.prototype.getRootNode()`       | 返回当前节点所在文档的根节点`document`，与`ownerDocument`一样 |

##### 2.1 Node.prototype.appendChild()

##### 2.2 Node.prototype.hasChildNodes()

##### 2.3 Node.prototype.cloneNode()

##### 2.4 Node.prototype.insertBefore()

##### 2.5 Node.prototype.removeChild()

##### 2.6 Node.prototype.replaceChild()

##### 2.7 Node.prototype.contains()

##### 2.8 Node.prototype.compareDocumentPosition()

##### 2.9 Node.prototype.isEqualNode()，Node.prototype.isSameNode()

##### 2.10 Node.prototype.normalize()

##### 2.11 Node.prototype.getRootNode()