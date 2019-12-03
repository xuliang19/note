#### 1. 概述

 事件发生以后，会产生一个事件对象，作为参数传给监听函数。浏览器原生提供一个`Event`对象，所有的事件都是这个对象的实例，或者说继承了`Event.prototype`对象。 

`Event`对象本身就是一个构造函数，可以用来生成新的实例。

```js
event = new Event(type, options);
```

`Event`构造函数接受两个参数。第一个参数`type`是字符串，表示事件的名称；第二个参数`options`是一个对象，表示事件对象的配置。该对象主要有下面两个属性。

- `bubbles`：布尔值，可选，默认为`false`，表示事件对象是否冒泡。
- `cancelable`：布尔值，可选，默认为`false`，表示事件是否可以被取消，即能否用`Event.preventDefault()`取消这个事件。一旦事件被取消，就好像从来没有发生过，不会触发浏览器对该事件的默认行为。

```js
var ev = new Event(
  'look',
  {
    'bubbles': true,
    'cancelable': false
  }
);
document.dispatchEvent(ev);
```

上面代码新建一个`look`事件实例，然后使用`dispatchEvent`方法触发该事件。

注意，如果不是显式指定`bubbles`属性为`true`，生成的事件就只能在“捕获阶段”触发监听函数。

```js
// HTML 代码为
// <div><p>Hello</p></div>
var div = document.querySelector('div');
var p = document.querySelector('p');

function callback(event) {
  var tag = event.currentTarget.tagName;
  console.log('Tag: ' + tag); // 没有任何输出
}

div.addEventListener('click', callback, false);

var click = new Event('click');
p.dispatchEvent(click);
```

上面代码中，`p`元素发出一个`click`事件，该事件默认不会冒泡。`div.addEventListener`方法指定在冒泡阶段监听，因此监听函数不会触发。如果写成`div.addEventListener('click', callback, true)`，那么在“捕获阶段”可以监听到这个事件。

另一方面，如果这个事件在`div`元素上触发。

```js
div.dispatchEvent(click);
```

那么，不管`div`元素是在冒泡阶段监听，还是在捕获阶段监听，都会触发监听函数。因为这时`div`元素是事件的目标，不存在是否冒泡的问题，`div`元素总是会接收到事件，因此导致监听函数生效。

#### 2. 实例属性

##### 2.1 Event.bubbles，Event.eventPhase

 `Event.bubbles`属性返回一个布尔值，表示事件是否冒泡，只读

 `Event.eventPhase`属性返回一个整数，表示事件所处阶段，只读

```js
var phase = event.eventPhase;
```

返回值有四种：

- 0：事件未发生
- 1：事件处于捕获阶段
- 2：事件处于目标阶段
- 3：事件处于冒泡阶段

##### 2.2 Event.cancelable，Event.cancelBubble，event.defaultPrevented

 `Event.cancelable`属性返回一个布尔值，表示事件是否可以取消，只读。

大多数浏览器原生事件可以取消，比如`click`事件，`Event`构造函数生成的事件默认不可取消，除非声明。

```js
var evt = new Event('foo');
evt.cancelable  // false
```

所以调用`Event.preventDefault()`时候最好用`Even.cancelable`判断一下是否可取消

```js
function preventEvent(event) {
  if (event.cancelable) {
    event.preventDefault();
  } else {
    console.warn('This event couldn\'t be canceled.');
    console.dir(event);
  }
}
```

`Event.cancelBubble`属性可设置布尔值，可读写。如果设为`true`，相当于执行`Event.stopPropagation()`，可以阻止事件的传播（效果完全一样，只是注意前者是个属性，后者是个函数，在使用的时候略有不同）

`Event.defaultPrevented`属性返回一个布尔值，表示该事件是否调用过`Event.preventDefault`方法，只读

```js
if (event.defaultPrevented) {
  console.log('该事件已经取消了');
}
```

##### 2.3 Event.currentTarget，Event.target

事件发生时，有两个与事件相关的节点，一个是事件的原始触发节点（`Event.target`），另一个是事件正在通过的节点（`Event.currentTarget`）

由于监听函数只有事件经过时才会触发，所以`Event.currentTarget`总是等同于监听函数内部的`this` 

##### 2.4 Event.type

`Event.type`属性返回一个字符串，表示事件类型，只读

```js
var evt = new Event('foo');
evt.type // "foo"
//click事件也会返回"click"
```

##### 2.5 Event.timeStamp

`Event.timeStamp`属性返回一个毫秒时间戳，表示事件发生的时间（比例如你按click从按住到松开的时间）。网页加载成功开始计算。（返回小数还是整数取决于浏览器）

```js
var evt = new Event('foo');
evt.timeStamp // 3683.6999999995896
```

可以用来计算鼠标移动速度

##### 2.6 Event.isTrusted

`Event.isTrusted`属性返回一个布尔值，表示该事件是否由真实的用户行为产生。比如，用户点击链接会产生一个`click`事件，该事件是用户产生的；`Event`构造函数生成的事件，则是脚本产生的。

```js
var evt = new Event('foo');
evt.isTrusted // false
```

上面代码中，`evt`对象是脚本产生的，所以`isTrusted`属性返回`false`。

##### 2.7 Event.detail

`Event.detail`属性只有浏览器的 UI （用户界面）事件才具有。该属性返回一个数值，表示事件的某种信息。具体含义与事件类型相关。比如，对于`click`和`dblclick`事件，`Event.detail`是鼠标按下的次数（`1`表示单击，`2`表示双击，`3`表示三击）；对于鼠标滚轮事件，`Event.detail`是滚轮正向滚动的距离，负值就是负向滚动的距离，返回值总是3的倍数。

```js
// HTML 代码如下
// <p>Hello</p>
function giveDetails(e) {
  console.log(e.detail);
}

document.querySelector('p').onclick = giveDetails;
```

#### 3. 实例方法

##### 3.1 Event.preventDefault()

`Event.preventDefault`方法取消浏览器对当前事件的默认行为。比如点击链接后，浏览器默认会跳转到另一个页面，使用这个方法以后，就不会跳转了。使用前提是事件对象的`cancelable`属性为`true`，否则没有效果

注意，该方法只是取消事件对**当前元素**的默认影响，**不会阻止事件的传播**

```js
// HTML 代码为
// <input type="checkbox" id="my-checkbox" />
var cb = document.getElementById('my-checkbox');

cb.addEventListener(
  'click',
  function (e){ e.preventDefault(); },
  false
);
```

上面取消了单击选中单选框这个行为，导致无法选中单选框

利用上述方法， 可以为文本输入框设置校验条件。如果用户的输入不符合条件，就无法将字符输入文本框

```js
// HTML 代码为
// <input type="text" id="my-input" />
var input = document.getElementById('my-input');
input.addEventListener('keypress', checkName, false);

function checkName(e) {
  if (e.charCode < 97 || e.charCode > 122) {
    e.preventDefault();
  }
}
```

上述文本框只能输入小写字母，其他字符的默认行为（按下键盘写入文本框）将被取消，非小写字母无法输入文本框中。

##### 3.2 Event.stopPropagation()

`stopPropagation`方法阻止事件在 DOM 中继续传播，防止再触发定义在别的节点上的监听函数，但是不包括在当前节点上其他的事件监听函数。

```js
function stopEvent(e) {
  e.stopPropagation();
}

el.addEventListener('click', stopEvent, false);
```

上面代码中，`click`事件将不会进一步冒泡到`el`节点的父节点

?> 理解：阻止的是你这个`click`动作的传播，监听函数仍在那里，所以同一个节点上同一事件设置多个监听函数，只需在第一个里面设置`even.stopPropagation();`就启用了`event`方法，阻止此节点`click`事件传播，**父元素设置了`stopPropagation`，其后代的`click`事件传播到父元素就被截断了**

##### 3.3 Event.stopImmediatePropagation()

 `Event.stopImmediatePropagation()`方法阻止事件传播，同一个节点定义的元素 ，比`Event.stopPropagation()`更彻底。

如果一个节点的同一事件（狭义来说就是`click`类事件）指定了多个监听函数，而其中有一个调用了 `Event.stopImmediatePropagation()`方法，其他监听函数就不会执行。但是不会影响其他事件（例如阻止了`click`事件传播，当不会影响除`click`之外的其他事件传播执行）的传播和执行。例如：

```js
el.addEventListener("click", function (event) {
    console.log("one");//不会执行
})
el.addEventListener("click", function (event) {
    console.log("two");//执行
    event.stopImmediatePropagation();
})
el.addEventListener("mouseover", function (event) {
    console.log("hover");//虽然是同一节点，但是是另一事件，会执行
})
```

##### 3.4 Event.composedPath()

`Event.composedPath()`返回一个数组，成员是事件的最底层节点和依次冒泡经过的所有上层节点（对象）

```js
// HTML 代码如下
// <div>
//   <p>Hello</p>
// </div>
var div = document.querySelector('div');
var p = document.querySelector('p');

div.addEventListener('click', function (e) {
  console.log(e.composedPath());
}, false);
// [p, div, body, html, document, Window]
```

