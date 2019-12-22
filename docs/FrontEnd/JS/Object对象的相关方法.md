#### 1. 常用方法

JavaScript 在`Object`对象上面，提供了很多相关方法，处理面向对象编程的相关操作，常用的如下：

- `Object.getPrototypeOf()`
- `Object.setPrototypeOf()`
- `Object.create()`
- `Object.prototype.isPrototypeOf()`
- `Object.getOwnPropertyNames()`
- `Object.prototype.hasOwnProperty()`

用法参考[标准库-Object对象](FrontEnd/JS/Object对象.md)

#### 2. Object.prototype.\_\_proto\_\_

实例对象的`__proto__`属性（前后各两个下划线），返回该对象的原型。该属性可读写（这是内部属性）

```js
var obj = {};
var p = {};
obj.__proto__ = p;
Object.getPrototypeOf(obj) === p // true
```

上面代码通过`__proto__`属性，将`p`对象设为`obj`对象的原型。

根据语言标准，`__proto__`属性只有浏览器才需要部署，其他环境可以没有这个属性。它前后的两根下划线，表明它本质是一个内部属性，不应该对使用者暴露。因此，应该尽量少用这个属性，而是用`Object.getPrototypeOf()`和`Object.setPrototypeOf()`，进行原型对象的读写操作。

原型链可以用`__proto__`很直观地表示（联想之前那张原型图）

```js
var A = {
  name: '张三'
};
var B = {
  name: '李四'
};
var proto = {
  print: function () {
    console.log(this.name);
  }
};
A.__proto__ = proto;
B.__proto__ = proto;
A.print() // 张三
B.print() // 李四
A.print === B.print // true
A.print === proto.print // true
B.print === proto.print // true
```

#### 3. 获取原型对象方法的比较

如前所述，`__proto__`属性指向当前对象的原型对象，即构造函数的`prototype`属性。

```js
var obj = new Object();
obj.__proto__ === Object.prototype
// true
obj.__proto__ === obj.constructor.prototype
// true
```

因此，获取实例对象`obj`的原型对象，有三种方法。

- `obj.__proto__`
- `obj.constructor.prototype`
- `Object.getPrototypeOf(obj)`

上面三种方法之中，前两种都不是很可靠。`__proto__`属性只有浏览器才需要部署，其他环境可以不部署。而`obj.constructor.prototype`在手动改变原型对象时，可能会失效。

```js
var P = function () {};
var p = new P();
var C = function () {};
//注意等于的是实例，而不是直接等于P.prototype,因为会改变原原型
C.prototype = p;
var c = new C();
c.constructor.prototype === p // false
```

上面代码中，构造函数`C`的原型对象被改成了`p`，但是实例对象的`c.constructor.prototype`却没有指向`p`。所以，在改变原型对象时，一般要同时设置`constructor`属性。

```js
C.prototype = p;
C.prototype.constructor = C;
var c = new C();
c.constructor.prototype === p // true
```

因此，推荐使用第三种`Object.getPrototypeOf`方法，获取原型对象。

#### 4. in 运算符和 for...in 循环

这两个都是不区分继承和自身的属性。`in`对不可遍历属性也有效，`for...in`只对可遍历属性有效

`in`运算符返回一个布尔值，表示一个对象是否具有某个属性（不区分是否为继承属性）。通常用于检查一个属性是否存在

```js
'length' in Date // true
'toString' in Date // true
```

获得对象的所有**可遍历**属性（不管自身的还是继承的），可以使用`for...in`循环

```js
var o1 = { p1: 123 };
var o2 = Object.create(o1, {
  p2: { value: "abc", enumerable: true }
});
for (p in o2) {
  console.info(p);
}
// p2
// p1
```

为了在`for...in`循环中获得对象自身的属性，可以采用`hasOwnProperty`方法判断一下。

```js
for ( var name in object ) {
  if ( object.hasOwnProperty(name) ) {
    /* loop code */
  }
}
```

获得对象的所有属性（不管是自身的还是继承的，也不管是否可遍历），可以使用下面的函数。

```js
function inheritedPropertyNames(obj) {
  var props = {};
  while(obj) {
    Object.getOwnPropertyNames(obj).forEach(function(p) {
      props[p] = true;
    });
    obj = Object.getPrototypeOf(obj);
  }
  return Object.getOwnPropertyNames(props);
}
```

下面是一个例子，列出`Date`对象的所有属性。

```js
inheritedPropertyNames(Date)
// [
//  "caller",
//  "constructor",
//  "toString",
//  "UTC",
//  ...
// ]
```

#### 4. 对象的拷贝

如果要拷贝一个对象，需要做到下面两件事情。

- 确保拷贝后的对象，与原对象具有同样的原型。
- 确保拷贝后的对象，与原对象具有同样的实例属性。

下面就是根据上面两点，实现的对象拷贝函数。

```js
function copyObject(orig) {
  var copy = Object.create(Object.getPrototypeOf(orig));
  copyOwnPropertiesFrom(copy, orig);
  return copy;
}
function copyOwnPropertiesFrom(target, source) {
  Object
    .getOwnPropertyNames(source)
    .forEach(function (propKey) {
      var desc = Object.getOwnPropertyDescriptor(source, propKey);
      Object.defineProperty(target, propKey, desc);
    });
  return target;
}
```

另一种更简单的写法，是利用 ES2017 才引入标准的`Object.getOwnPropertyDescriptors`方法。

```js
function copyObject(orig) {
  return Object.create(
    Object.getPrototypeOf(orig),
    Object.getOwnPropertyDescriptors(orig)
  );
}
```