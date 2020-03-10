事件的本质是程序各个组成部分之间的一种通信方式，也是异步编程的一种实现

#### 1. 概述

 DOM 的事件操作（监听和触发），都定义在`EventTarget`接口； 其它一些需要事件通信的浏览器内置对象（比如，`XMLHttpRequest`、`AudioNode`、`AudioContext`）也部署了这个接口 

该接口主要提供三个实例方法。

- `addEventListener`：绑定事件的监听函数
- `removeEventListener`：移除事件的监听函数
- `dispatchEvent`：触发事件

#### 2. EventTarget.addEventListener()

 `EventTarget.addEventListener()`用于在当前节点或对象上，定义一个特定事件的监听函数。一旦这个事件发生，就会执行监听函数。该方法没有返回值

```js
target.addEventListener(type, listener[, useCapture]);
```

该方法接受三个参数。

- `type`：事件名称，大小写敏感（不需要加`on`）
- `listener`：监听函数（还可以是一个对象）。事件发生时，会调用该监听函数。
- `useCapture`：布尔值，表示监听函数是否在捕获阶段（capture）触发，默认为`false`（监听函数只在冒泡阶段被触发），该参数可选。

例如：

```js
function hello() {
  console.log('Hello world');
}

var button = document.getElementById('btn');
button.addEventListener('click', hello, false);
```

上面代码中，`button`节点的`addEventListener`方法绑定`click`事件的监听函数`hello`，该函数只在冒泡阶段触发。 

 **关于参数，有两个地方需要注意：**

- 第二个参数除了监听函数，还可以是一个具有`handleEvent`方法的对象。例：

  ```js
  buttonElement.addEventListener('click', {
    handleEvent: function (event) {
      console.log('click');
    }
  });
  ```

- 第三个参数除了布尔值`useCapture`，还可以是一个属性配置对象。该对象有以下属性 

  > - `capture`：布尔值，表示该事件是否在`捕获阶段`触发监听函数。
  > - `once`：布尔值，表示监听函数是否只触发一次，然后就自动移除。
  > - `passive`：布尔值，表示监听函数不会调用事件的`preventDefault`方法。如果监听函数调用了，浏览器将忽略这个要求，并在监控台输出一行警告。

  例如，如果希望监听函数只执行一次，用`once`

  ```js
  element.addEventListener('click', function (event) {
    // 只执行一次的代码
  }, {once: true});
  ```

 `addEventListener` 可以针对当前对象的同一事件，添加多个不同的监听函数，**函数按排列顺序执行**（ 为同一个事件多次添加同一个监听函数，该函数只会执行一次 ，多余的会自动移除， 无须`removeEventListener移除 ）

如果希望向监听函数传递参数，可以**用匿名函数包装一下监听函数**，（不能直接`print("hello")`，这样就直接调用函数了，一打开页面就会触发，`()`具有调用函数的作用，要么写`print`，要么`function [函数名]() { }`）例：

```js
function print(x) {
  console.log(x);
}

var el = document.getElementById('div1');
el.addEventListener('click', function () { print('Hello'); }, false);
```

事件处理程序内部，对象`this` 始终等于`currentTarget` ，就是事件所在节点，而`event.target`指向的是触发事件的最初节点。

#### 3. Event.Target.removeEventListener()

`EventTarget.removeEventListener`方法用来移除`addEventListener`方法添加的事件监听函数。该方法没有返回值。

注意，`EventTarget.removeEventListener`无法移除匿名函数；第三个参数不同也不行。

#### 4. EventTarget.dispatchEvent()
`EventTarget.dispatchEvent`方法在当前节点上触发指定事件，从而触发监听函数的执行。该方法返回一个布尔值，只要有一个监听函数调用了`Event.preventDefault()`，则返回值为`false`，否则为`true`。 

 `dispatchEvent`方法的参数是一个`Event`对象的实例 

```js
para.addEventListener('click', hello, false);
var event = new Event('click');
para.dispatchEvent(event);
```

上面代码在当前节点触发了`click`事件。

如果`dispatchEvent`方法的参数为空，或者不是一个有效的事件对象，将报错。

下面代码根据`dispatchEvent`方法的返回值，判断事件是否被取消了。

```js
var canceled = !cb.dispatchEvent(event);
if (canceled) {
  console.log('事件取消');
} else {
  console.log('事件未取消');
}
```