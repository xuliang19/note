#### 1. 构造函数

`Array`是 JavaScript 的原生对象，同时也是一个构造函数，可以用它生成新的数组（**通常使用字面的方法来生成**）

有无`new`，运行结果一样

```js
var arr = new Array(2);
// 等同于
var arr = Array(2);
```

`Array`构造函数有一个很大的缺陷，就是不同的参数，会导致它的行为不一致

```js
// 无参数时，返回一个空数组
new Array() // []
// 单个正整数参数，表示返回的新数组的长度
new Array(1) // [ empty ]
new Array(2) // [ empty x 2 ]
// 非正整数的数值作为参数，会报错
new Array(3.2) // RangeError: Invalid array length
new Array(-3) // RangeError: Invalid array length
// 单个非数值（比如字符串、布尔值、对象等）作为参数，
// 则该参数是返回的新数组的成员
new Array('abc') // ['abc']
new Array([1]) // [Array[1]]
// 多参数时，所有参数都是返回的新数组的成员
new Array(1, 2) // [1, 2]
new Array('a', 'b', 'c') // ['a', 'b', 'c']
```

所以，使用字面量生成新数组是更好的做法

```js
// bad
var arr = new Array(1, 2);
// good
var arr = [1, 2];
```

空值和设置为undefined是不一样的，空值虽然可以取到`length`但是取不到键名

```js
var a = new Array(3);
var b = [undefined, undefined, undefined];
a.length // 3
b.length // 3
a[0] // undefined
b[0] // undefined
0 in a // false
0 in b // true
```



#### 2. 静态方法

##### 2.1 Array.isArray()

#### 3. 实例方法