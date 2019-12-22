#### 1. 概述

JS中所有其他对象都继承`Object`对象（都是`Object`实例）。`Object`中方法分为两类：`Object`本身方法和`Object`实例方法

###### （1）`Object`对象本身方法

就是直接定义在`Object`对象的方法。

```js
Object.print = function (o) { console.log(o) };
```

上面代码中，`print`方法就是直接定义在`Object`对象上

###### （2）`Object`的实例方法

就是定义在`Object`原型对象`Object.prototype`上的方法，可以被`Object`的实例直接使用

```js
Object.prototype.print = function () {
  console.log(this);
};
var obj = new Object();
obj.print() // Object
```

上述代码中，`obj`对象的`print`方法实质上就是调用`Object.prototype.print`方法（定义在`Object.prototype`对象上面的属性和方法，将被所有实例对象共享）

#### 2. Object()

`Object`本身是一个函数，可以当作工具方法使用，将任意值转为对象（常用于确保某个值一定是对象）。如果参数为空（或者为`undefined`和`null`），`Object()`返回一个空对象

`instanceof`运算符用来验证一个对象是否为某个构造函数的实例

参数为原始值返回原始值的包装对象，参数是对象则返回该对象（注: 函数, 数组也是对象）

```js
var obj = Object(true);
obj instanceof Object // true
obj instanceof Boolean // true

var fn = function () {};
var obj = Object(fn); // 返回原函数
obj === fn // true
```

利用这一点，可以判断一个变量是否为对象

```js
function isObject(value) {
  return value === Object(value);
}
isObject([]) // true
isObject(true) // false
```

#### 3. Object 构造函数

`Object`不仅可以当作工具函数使用，还可以当作构造函数使用，来通过它生成新对象（不过一般都是用字面量的方法`var obj = {}`）

```js
var obj = new Object();
```

`Object`作为构造函数的用法和上述差不多，参数为原始值返回原始值的包装对象，参数是对象则返回该对象

#### 4. Object 的静态方法

静态方法指的就是`Object`对象自身的方法：

###### （1）遍历对象属性相关方法

|              方法              |                 作用                 |
| :----------------------------: | :----------------------------------: |
|        `Object.keys()`         |    遍历对象属性（返回可遍历属性）    |
| `Object.getOwnPropertyNames()` | 遍历对象属性（还可返回不可遍历属性） |

###### （2）属性的描述对象相关方法

|                方法                 |               作用               |
| :---------------------------------: | :------------------------------: |
| `Object.getOwnPropertyDescriptor()` |        获取属性的描述对象        |
|      `Object.defineProperty()`      | 通过描述对象，定义或修改一个属性 |
|     `Object.defineProperties()`     | 通过描述对象，定义或修改多个属性 |

###### （3）控制对象状态的方法

|             方法             |                       作用                       |
| :--------------------------: | :----------------------------------------------: |
| `Object.preventExtensions()` |                禁止对象添加新属性                |
|   `Object.isExtensible()`    |                判断是否可添加属性                |
|       `Object.seal()`        |        禁止对象配置（无法添加或删除属性）        |
|     `Object.isSealed()`      |                判断对象是否可配置                |
|      `Object.freeze()`       | 冻结对象（无法添加或删除属性，也无法改变属性值） |
|     `Object.isFrozen()`      |                 判断对象是否冻结                 |

一个比一个严格。

###### （4）原型链相关方法

|           方法            |                作用                |
| :-----------------------: | :--------------------------------: |
|     `Object.create()`     | 指定原型对象和属性，返回一个新对象 |
| `Object.getPrototypeOf()` |     获取对象的`Prototype`对象      |
| `Object.setPrototypeOf()` |    为参数设置原型，返回参数对象    |

##### 4.1 Object.keys()，Object.getOwnPropertyNames()

`Object.keys()`，`Object.getOwnPropertyNames()`方法都用来遍历对象的属性。均接受一个对象作为参数，返回一个数组（包含对象的属性名，不包括继承的属性名）

不同点在于`Object.keys()`只返回可遍历的属性，而`Object.getOwnPropertyNames()`还返回不可遍历属性。例如数组的`length`属性使不可遍历属性

```js
var a = ['Hello', 'World'];
Object.keys(a) // ["0", "1"]
Object.getOwnPropertyNames(a) // ["0", "1", "length"]
```

还可以用来给对象属性计数（对象是没有`length`属性的，数组才有）

```js
Object.keys(obj).length
```

?> 一般情况下都是用`Object.keys`方法遍历对象的属性

##### 4.2 Object.getOwnPropertyDescriptor()

获取属性的描述对象（只能用于对象自身的属性，继承的无法获取），第一个参数为目标对象，第二个参数为属性名

```js
var obj = { p: 'a' };
Object.getOwnPropertyDescriptor(obj, 'p')
// Object { value: "a",
//   writable: true,
//   enumerable: true,
//   configurable: true
// }
```

##### 4.3 Object.defineProperty()，Object.defineProperties()

通过属性描述对象，定义或修改一个属性，返回修改后的对象

```js
Object.defineProperty(object, propertyName, attributesObject)
```

- object：属性所在的对象
- propertyName：字符串，表示属性名
- attributesObject：属性描述对象

例如：

```js
var obj = Object.defineProperty({}, 'p', {
  value: 123,
  writable: false,
  enumerable: true,
  configurable: false
});
obj.p // 123
obj.p = 246;
obj.p // 123
//因为value设置为了不可写
```

`p`属性直接定义在这个空对象上面，然后返回这个对象，这是`Object.defineProperty()`的常见用法（如果属性存在，则覆盖该属性描述对象）

一次性修改多个属性的描述对象，则用 `Object.defineProperties()` 方法

```js
var obj = Object.defineProperties({}, {
  p1: { value: 123, enumerable: true },
  p2: { value: 'abc', enumerable: true },
  p3: { get: function () { return this.p1 + this.p2 },
    enumerable:true,
    configurable:true
  }
});
obj.p1 // 123
obj.p2 // "abc"
obj.p3 // "123abc"
```

`p3`属性定义了取值函数`get`，即每次读取该属性都会调用这个取值函数

!> 注：设置了`get`，则`writable`不能设为`true`，也不能同时定义`value`属性，否则报错

`Object.defineProperty()`和`Object.defineProperties()`参数里面的属性描述对象，`writable`、`configurable`、`enumerable`这三个属性的默认值都为`false`

```js
//用一个空的属性描述对象，就可以看到各个元属性的默认值
var obj = {};
Object.defineProperty(obj, 'foo', {});
Object.getOwnPropertyDescriptor(obj, 'foo')
// {
//   value: undefined,
//   writable: false,
//   enumerable: false,
//   configurable: false
// }
```

##### 4.4 Object.preventExtensions()

使得一个对象无法再添加新的属性

```js
var obj = new Object();
Object.preventExtensions(obj);
Object.defineProperty(obj, 'p', {
  value: 'hello'
});
// TypeError: Cannot define property:p, object is not extensible.
obj.p = 1;
obj.p // undefined
```

##### 4.5 Object.isExtensible()

判断对象是否使用`Object.preventExtensions`（即判断是否可以为对象添加属性）

```js
var obj = new Object();
Object.isExtensible(obj) // true
Object.preventExtensions(obj);
Object.isExtensible(obj) // false
```

##### 4.6 Object.seal()

`Object.seal`方法使得一个对象既无法添加新属性，也无法删除旧属性

```js
var obj = { p: 'hello' };
Object.seal(obj);
delete obj.p;
obj.p // "hello"
obj.x = 'world';
obj.x // undefined
```

`Object.seal`方法实质是把属性描述对象的 `configurable`属性设为`false` 。`Object.seal`不会禁止修改某个属性的值

```js
var obj = { p: 'a' };
Object.seal(obj);
obj.p = 'b';
obj.p // 'b'
```

##### 4.7 Object.isSealed()

判断对象是否使用了`Object.seal`方法

```js
var obj = { p: 'a' };

Object.seal(obj);
Object.isSealed(obj) // true
Object.isExtensible(obj) // false
```

用了`Object.seal`之后，`Object.isExtensible`会返回`false` 

##### 4.8 Object.freeze()

`Object.freeze`使对象无法添加或删除属性，也无法改变属性值（这个对象实际上变成了常量）

```js
var obj = {
  p: 'hello'
};
Object.freeze(obj);
obj.p = 'world';
obj.p // "hello"
obj.t = 'hello';
obj.t // undefined
delete obj.p // false
obj.p // "hello"
```

##### 4.9 Object.isFrozen()

`Object.isFrozen`判断对象是否使用了`Object.freeze`方法

```js
var obj = {
  p: 'hello'
};
Object.freeze(obj);
Object.isFrozen(obj) // true
```

用了`Object.freeze`方法以后，`Object.isSealed`将会返回`true`，`Object.isExtensible`返回`false` 

##### 4.10 局限性

**局限1：**上面三个方法锁定对象有一个漏洞：可以通过改变原型对象，来为对象增加属性

```js
var obj = new Object();
Object.preventExtensions(obj);
var proto = Object.getPrototypeOf(obj);
proto.t = 'hello';
obj.t
// hello
```

可行的解决办法是把`obj`的原型也冻结住

```js
var obj = new Object();
Object.preventExtensions(obj);
var proto = Object.getPrototypeOf(obj);
Object.preventExtensions(proto);
proto.t = 'hello';
obj.t // undefined
```

**局限2：**如果属性值是对象，上面方法只能冻结属性指向的对象（冻结地址），无法冻结对象的内容

```js
var obj = {
  foo: 1,
  bar: ['a', 'b']
};
Object.freeze(obj);
obj.bar.push('c');
obj.bar // ["a", "b", "c"]
```

##### 4.11 Object.create()

有时候我们无法取得构造函数，只有实例对象，此时想创建另一个实例可用`Object.create`。该方法接受一个对象为参数，以它为原型返回一个实例对象，该实例完全继承原型对象的属性

```js
// 原型对象
var A = {
  print: function () {
    console.log('hello');
  }
};
// 实例对象
var B = Object.create(A);
Object.getPrototypeOf(B) === A // true
B.print() // hello
B.print === A.print // true
```

实际上，`Object.create`方法可以用下面的代码代替。

```js
if (typeof Object.create !== 'function') {
  Object.create = function (obj) {
    function F() {}
    F.prototype = obj;
    return new F();
  };
}
```

上面代码表明，`Object.create`方法的实质是新建一个空的构造函数`F`，然后让`F.prototype`属性指向参数对象`obj`，最后返回一个`F`的实例，从而实现让该实例继承`obj`的属性。

下面三种方式生成的新对象是等价的。

```js
var obj1 = Object.create({});
var obj2 = Object.create(Object.prototype);
var obj3 = new Object();
```

如果想要生成一个不继承任何属性（比如没有`toString`和`valueOf`方法）的对象，可以将`Object.create`的参数设为`null`。

```js
var obj = Object.create(null);
obj.valueOf()
// TypeError: Object [object Object] has no method 'valueOf'
```

上面代码中，对象`obj`的原型是`null`，它就不具备一些定义在`Object.prototype`对象上面的属性，比如`valueOf`方法。

使用`Object.create`方法的时候，必须提供对象原型，即参数不能为空，或者不是对象，否则会报错。

```js
Object.create()
// TypeError: Object prototype may only be an Object or null
Object.create(123)
// TypeError: Object prototype may only be an Object or null
```

`Object.create`方法生成的新对象，动态继承了原型。在原型上添加或修改任何方法，会立刻反映在新对象之上。

除了对象的原型，`Object.create`方法还可以接受第二个参数。该参数是一个属性描述对象，它所描述的对象属性，会添加到实例对象，作为该对象自身的属性。

```js
var obj = Object.create({}, {
  p1: {
    value: 123,
    enumerable: true,
    configurable: true,
    writable: true,
  },
  p2: {
    value: 'abc',
    enumerable: true,
    configurable: true,
    writable: true,
  }
});
// 等同于
var obj = Object.create({});
obj.p1 = 123;
obj.p2 = 'abc';
```

`Object.create`方法生成的对象，继承了它的原型对象的构造函数（记住那张图）

```js
function A() {}
var a = new A();
var b = Object.create(a);
b.constructor === A // true
b instanceof A // true
```

##### 4.12 Object.getPrototypeOf()

`Object.getPrototypeOf`方法返回参数对象的原型。这是获取原型对象的标准方法。

```js
var F = function () {};
var f = new F();
Object.getPrototypeOf(f) === F.prototype // true
```

上面代码中，实例对象`f`的原型是`F.prototype`。

下面是几种特殊对象的原型。

```js
// 空对象的原型是 Object.prototype
Object.getPrototypeOf({}) === Object.prototype // true
// Object.prototype 的原型是 null
Object.getPrototypeOf(Object.prototype) === null // true
// 函数的原型是 Function.prototype
function f() {}
Object.getPrototypeOf(f) === Function.prototype // true
```

##### 4.13 Object.setPrototypeOf()

`Object.setPrototypeOf`方法为参数对象设置原型，返回该参数对象。它接受两个参数，第一个是现有对象，第二个是原型对象。

```js
var a = {};
var b = {x: 1};
Object.setPrototypeOf(a, b);
Object.getPrototypeOf(a) === b // true
a.x // 1
```

上面代码中，`Object.setPrototypeOf`方法将对象`a`的原型，设置为对象`b`，因此`a`可以共享`b`的属性。

`new`命令可以使用`Object.setPrototypeOf`方法模拟。

```js
var F = function () {
  this.foo = 'bar';
};
var f = new F();
// 等同于
var f = Object.setPrototypeOf({}, F.prototype);
F.call(f);
```

和之前的`new`原理结合看，还有构造函数的继承

#### 5. Object 的实例方法

除了上述的静态方法，还有定义在`Object.prototype`的方法（实例方法），所有`Object`的实例对象都继承了这些方法，主要有以下六个：

|                 实例方法                  |              作用              |
| :---------------------------------------: | :----------------------------: |
|       `Object.prototype.valueOf()`        |        返回对象对应的值        |
|       `Object.prototype.toString()`       |    返回对象对应的字符串形式    |
|    `Object.prototype.toLocaleString()`    |  返回对象对应的本地字符串形式  |
|    `Object.prototype.hasOwnProperty()`    |   判断属性是自身的还是继承的   |
|    `Object.prototype.isPrototypeOf()`     | 判断对象是否为另一个对象的原型 |
| `Object.prototype.propertyIsEnumerable()` |       判断属性是否可遍历       |

##### 5.1 Object.prototype.valueOf()

`valueOf`方法的作用是返回一个对象的“值”，默认情况下返回对象本身（除非自定义返回数值）。主要用途是类型转换时会默认调用这个方法

```js
var obj = new Object();
obj.valueOf() === obj // true

var obj = new Object();
1 + obj // "1[object Object]"
//自定义valueOf方法
var obj = new Object();
obj.valueOf = function () {
  return 2;
};
1 + obj // 3
//这样就覆盖了继承的Object.prototype.valueOf
```

##### 5.2 Object.prototype.toString()

`toString`方法的作用是返回一个对象的字符串形式，默认情况下返回类型字符串（没什么用处）

```js
var o2 = {a:1};
o2.toString() // "[object Object]"
```

主要作用通过自定义`toString`方法让对象在类型转换时得到指定字符串

```js
var obj = new Object();
obj.toString = function () {
  return 'hello';
};
obj + ' ' + 'world' // "hello world"
```

数组、字符串、函数、Date对象都分别自定义了自己的`toString`方法，覆盖了继承的`Object.prototype.toString`

```js
[1, 2, 3].toString() // "1,2,3"

'123'.toString() // "123"

(function () {
  return 123;
}).toString()
// "function () {
//   return 123;
// }"

(new Date()).toString()
// "Tue May 10 2016 09:11:31 GMT+0800 (CST)"
```

##### 5.3 toString() 的应用：判断数据类型

返回的`[object Object]`类型字符串中，第二个`Object`表示该值的构造函数。可以用来判断数据类型，由于实例对象可能会自定义`toString`方法，为了得到类型字符串，应直接使用`Object.prototype.toString`方法。通过函数的`call`方法，可以在任意值上调用这个方法。

```js
Object.prototype.toString.call(value)
```

不同数据类型的`Object.prototype.toString`返回值如下：

|     类型      | 返回                 |
| :-----------: | -------------------- |
|     数值      | `[object Number]`    |
|    字符串     | `[object String]`    |
|    布尔值     | `[object Boolean]`   |
|   undefined   | `[object Undefined]` |
|     null      | `[object Null]`      |
|     数组      | `[object Array]`     |
| arguments对象 | `[object Arguments]` |
|     函数      | `[object Function]`  |
|   Error对象   | `[object Error]`     |
|   Date对象    | `[object Date]`      |
|  RegExp对象   | `[object RegExp]`    |
|   其他对象    | `[object Object]`    |

利用这个特性，可以用来准确判断数据类型（比`typeof`精确多了）

```js
var type = function (o){
  var s = Object.prototype.toString.call(o);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};

type({}); // "object"
type([]); // "array"
type(5); // "number"
type(null); // "null"
type(); // "undefined"
type(/abcd/); // "regex"
type(new Date()); // "date"
```

##### 5.4 Object.prototype.toLocaleString()

和`toString`返回的结果相同，主要作用是留出一个接口，让不同的对象实现自己版本的`toLocaleString`（根据地域的不同）

目前主要有三个对象自定义了`toLocalString`方法

- `Array.prototype.toLocaleString()`
- `Number.prototype.toLocaleString()`
- `Date.prototype.toLocaleString()`

例如，日期的实例对象`toString`和`toLocaleString`返回值就不一样，后者和用户所设定的地域有关

```js
var date = new Date();
date.toString() // "Tue Jan 01 2018 12:01:33 GMT+0800 (CST)"
date.toLocaleString() // "1/01/2018, 12:01:33 PM"
```

##### 5.5 Object.prototype.hasOwnProperty()

接受一个字符串作为参数，返回布尔值。表示该实例对象是否具有该属性（不包括继承的）

```js
var obj = {
  p: 123
};
obj.hasOwnProperty('p') // true
obj.hasOwnProperty('toString') // false
```

##### 5.6 Object.prototype.isPrototypeOf()

实例对象的`isPrototypeOf`方法，用来判断该对象是否为参数对象的原型。

```js
var o1 = {};
var o2 = Object.create(o1);
var o3 = Object.create(o2);
o2.isPrototypeOf(o3) // true
o1.isPrototypeOf(o3) // true
```

上面代码中，`o1`和`o2`都是`o3`的原型。这表明只要实例对象处在参数对象的原型链上，`isPrototypeOf`方法都返回`true`。

```js
Object.prototype.isPrototypeOf({}) // true
Object.prototype.isPrototypeOf([]) // true
Object.prototype.isPrototypeOf(/xyz/) // true
Object.prototype.isPrototypeOf(Object.create(null)) // false
```

上面代码中，由于`Object.prototype`处于原型链的最顶端，所以对各种实例都返回`true`，只有直接继承自`null`的对象除外。

##### 5.7 Object.prototype.propertyIsEnumerable()

返回一个布尔值，用来判断某个属性是否可遍历（只能用于判断对象自身的属性，对于对象继承的一律返回`false`）

```js
var obj = {};
obj.p = 123;
obj.propertyIsEnumerable('p') // true
obj.propertyIsEnumerable('toString') // false
```

