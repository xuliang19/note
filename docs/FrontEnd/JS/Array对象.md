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

返回布尔值，判断参数是否为数组。可弥补`typeof`的不足

```js
var arr = [1, 2, 3];
typeof arr // "object"
Array.isArray(arr) // true
```

#### 3. 实例方法

有以下实例方法

|          实例方法           |                             作用                             |
| :-------------------------: | :----------------------------------------------------------: |
|  `valueOf()`,`toString()`   |           前者返回数组本身，后者返回数组字符串形式           |
|      `push()`,`pop()`       | 前者在数组末端添加一个或多个元素，并返回添加后的数组的长度（该方法会改变原数组）；后者删除数组最后一个元素，并返回该元素（该方法会改变原数组长度） |
|    `shift()`,`unshift()`    | 前者删除数组第一个元素，并返回该元素（该方法会改变原数组）；后者在数组第一个位置添加元素，并返回添加后的数组长度 |
|          `join()`           | 指定参数作为分隔符，将所有数组成员连接成一个字符串返回。默认使用逗号 |
|         `concat()`          | 将多个数组合并，将新数组成员添加原数组后面，返回一个新数组（该方法不会改变原数组） |
|         `reverse()`         |            颠倒数组成员排列（该方法将改变原数组）            |
|          `slice()`          |  提取目标数组一部分，返回一个新数组（该方法不会改变原数组）  |
|         `splice()`          | 删除原数组的一部分成员，并可在删除位置添加新的数组成员，返回被删除的成员（该方法会改变原数组） |
|          `sort()`           |  对数组进行排序，默认按照字典顺序排序（该方法会改变原数组）  |
|           `map()`           | 将数组所有成员依次传入参数函数，把执行结果组成一个新的数组返回（该方法不会改变原数组） |
|         `forEach()`         | 和`map()`类似，对所有成员一次执行参数函数，但不返回值，只用来操作数据 |
|         `filter()`          | 过滤数组成员，满足条件的成员组成一个新数组返回（该方法不会改变原数组） |
|     `some()`,`every()`      | 判断数组成员是否符合某种条件，返回布尔值。前者只要有一个成员返回`true`，则返回`true`；后者是所有成员返回`true`，则返回`true` |
| `reduce()`,`reduceRight()`  | 依次处理数组的每一个成员，最终累计为一个值。前者从左到右处理；后者从右到左处理 |
| `indexOf()`,`lastIndexOf()` | 前者返回给定元素在数组中第一次出现的位置，没有出现则返回`-1`；后者返回给定元素在数组中最后一次出现的位置，没有出现则返回`-1` |

##### 3.1 valueOf()，toString()

`valueOf`返回数组本身

```js
var arr = [1, 2, 3];
arr.valueOf()//[1, 2, 3]
```

`toString`返回数组的字符串形式

```js
var arr = [1, 2, 3];
arr.toString() // "1,2,3"
var arr = [1, 2, 3, [4, 5, 6]];
arr.toString() // "1,2,3,4,5,6"
```

##### 3.2 push()，pop()

`push`在数组末尾添加一个或多个元素，返回添加后的数组长度（会改变原数组）

```js
var arr = [];
arr.push(1) // 1
arr.push('a') // 2
arr.push(true, {}) // 4
arr // [1, 'a', true, {}]
```

`pop`删除数组最后一个元素，并返回该元素（会改变原数组）。若是空数组返回`undefined`

```js
var arr = ['a', 'b', 'c'];
arr.pop() // 'c'
arr // ['a', 'b']
[].pop()//undefined
```

`push`和`pop`结合使用，构成了“后进先出”的栈结构

```js
var arr = [];
arr.push(1, 2);
arr.push(3);
arr.pop();
arr // [1, 2]
```

上述`3`最后进入数组，但是最早离开数组

##### 3.3 shift()，unshift()

`shift`删除数组第一个元素，并返回该元素（会改变原数组）

```js
var a = ['a', 'b', 'c'];
a.shift() // 'a'
a // ['b', 'c']
```

`push`和`shift`结合使用，构成了“先进先出”的队列结构

`unshift`在数组首位添加一个或多个元素，并返回添加后的数组长度（会改变原数组）

```js
var arr = [ 'c', 'd' ];
arr.unshift('a', 'b') // 4
arr // [ 'a', 'b', 'c', 'd' ]
```

##### 3.4 join()

`join`指定参数作为分隔符，将所有数组成员连接成一个字符串返回，默认有逗号分隔

```js
var a = [1, 2, 3, 4];
a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"
a.join() // "1,2,3,4"
```

如果数组成员是`undefined`或`null`或空位，会被转成空字符串。

```js
[undefined, null].join('#')
// '#'
['a',, 'b'].join('-')
// 'a--b'
```

通过`call`方法，这个方法也可以用于字符串或类似数组的对象。

```js
Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"
var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')
// 'a-b'
```

##### 3.5 concat()

`concat`将新数组的成员添加到原数组的末尾，返回一个新数组（不会改变原数组）

```js
['hello'].concat(['world'])
// ["hello", "world"]
['hello'].concat(['world'], ['!'])
// ["hello", "world", "!"]
[].concat({a: 1}, {b: 2})
// [{ a: 1 }, { b: 2 }]
```

除了数组，`concat`还可用其他类型的值作为参数

```js
[1, 2, 3].concat(4, 5, 6)
// [1, 2, 3, 4, 5, 6]
```

 如果数组成员包括对象，`concat`方法返回当前数组的一个浅拷贝（拷贝的是对象的引用）

##### 3.6 reverse()

`reverse`用于颠倒数组排列，返回改变后的数组（会改变原数组）

```js
var a = ['a', 'b', 'c'];
a.reverse() // ["c", "b", "a"]
a // ["c", "b", "a"]
```

##### 3.7 slice()

`slice`用于提取数组的一部分，返回一个新数组（不会改变原数组）

```js
arr.slice(start, end);
```

为左闭右开；第二参数省略则默认为最后一个；没有参数则返回原数组的拷贝

```js
var a = ['a', 'b', 'c'];
a.slice(0) // ["a", "b", "c"]
a.slice(1) // ["b", "c"]
a.slice(1, 2) // ["b"]
a.slice(2, 6) // ["c"]
a.slice() // ["a", "b", "c"]
```

参数为负数表示对应的倒数位置，`-2`表示倒数第二个位置。若第一个大于第二个参数，则返回空数组

```js
var a = ['a', 'b', 'c'];
a.slice(-2, -1) // ["b"]
a.slice(4) // []
a.slice(2, 1) // []
```

`slice`一个重要的应用就是将类似数组的对象转为真正的数组

```js
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']
Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```

##### 3.8 splice()

`splice`删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回被删除的元素（会改变原数组）

```js
arr.splice(start, count, addElement1, addElement2, ...);
```

`splice`第一个参数是删除的起始位置（负数则表示从倒数开始），第二个是删除的个数。后面的则是要插入数组的新成员（任意个）

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2, 1, 2) // ["e", "f"]
a // ["a", "b", "c", "d", 1, 2]
```

如果只是插入元素，`splice`第二个参数可设为`0`

```js
var a = [1, 1, 1];
a.splice(1, 0, 2) // []
a // [1, 2, 1, 1]
```

只提供一个参数，则相当于把原数组在指定位置拆分为两个数组

```js
var a = [1, 2, 3, 4];
a.splice(2) // [3, 4]
a // [1, 2]
```

##### 3.9 sort()

`sort`对数组成员按字典排序（会改变原数组）

```js
['d', 'c', 'b', 'a'].sort()
// ['a', 'b', 'c', 'd']
[4, 3, 2, 1].sort()
// [1, 2, 3, 4]
//数值会先转为字符串，再按照字典顺序比较
```

`sort`还可接受一个函数作为参数传入，函数接受两个参数，如果返回值大于`0`，则`a`在`b`后面（升序），其他情况`a`在`b`后面。

```js
[10111, 1101, 111].sort(function (a, b) {
  return a - b;
})
// [111, 1101, 10111]
//若想改为降序，则可以 return b-a;
```

`sort`的参数函数本身接受两个参数，表示进行比较的两个数组成员。如果该函数的返回值大于`0`，表示第一个成员排在第二个成员后面；其他情况下，都是第一个元素排在第二个元素前面

```js
[
  { name: "张三", age: 30 },
  { name: "李四", age: 24 },
  { name: "王五", age: 28  }
].sort(function (o1, o2) {
  return o1.age - o2.age;
})
// [
//   { name: "李四", age: 24 },
//   { name: "王五", age: 28  },
//   { name: "张三", age: 30 }
// ]
```

注意，自定义的排序函数应该返回数值，否则不同的浏览器可能有不同的实现，不能保证结果都一致

##### 3.10 map()

`map`将数组的所有成员依次传入参数函数，再把每次执行的结果组成一个新数组返回（不会改变原数组）

```js
var numbers = [1, 2, 3];
numbers.map(function (n) {
  return n + 1;
});
// [2, 3, 4]
numbers
// [1, 2, 3]
```

`map`方法接受一个函数作为参数。该函数调用时，`map`方法向它传入三个参数：`elem`为当前成员的值，`index`为当前成员的位置，`arr`为原数组

```js
[1, 2, 3].map(function(elem, index, arr) {
  return elem * index;
});
// [0, 2, 6]
```

`map`方法还可以接受第二个参数，用来绑定回调函数内部的`this`变量

```js
var arr = ['a', 'b', 'c'];
[1, 2].map(function (e) {
  return this[e];
}, arr)
// ['b', 'c']
```

如果数组有空位，`map`方法的回调函数在这个位置不会执行，会跳过数组的空位（但是不会跳过`undefined`和`null`）

```js
var f = function (n) { return 'a' };
[1, undefined, 2].map(f) // ["a", "a", "a"]
[1, null, 2].map(f) // ["a", "a", "a"]
[1, , 2].map(f) // ["a", , "a"]
```

##### 3.11 forEach()

`forEach`和`map`方法类似，但是`forEach`方法不返回值，只用来操作数据（如果数组遍历的目的是为了得到返回值，那么使用`map`方法，否则使用`forEach`方法）

```js
function log(element, index, array) {
  console.log('[' + index + '] = ' + element);
}
[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```

上面代码中，`forEach`遍历数组不是为了得到返回值，而是为了在屏幕输出内容，所以不必使用`map`方法

`forEach`方法也可以接受第二个参数，绑定参数函数的`this`变量

`forEach`无法中断执行，总会将所有成员遍历完。若想中断循环可使用`for`循环

```js
var arr = [1, 2, 3];
for (var i = 0; i < arr.length; i++) {
  if (arr[i] === 2) break;
  console.log(arr[i]);
}
// 1
```

`forEach`同样会跳过数组的空位（不会跳过`undefined`和`null`）

##### 3.12 filter()

`filter`过滤数组成员，满足条件的成员组成新数组返回（不会改变原数组）

```
[1, 2, 3, 4, 5].filter(function (elem) {
  return (elem > 3);
})
// [4, 5]

var arr = [0, 1, 'a', false];
arr.filter(Boolean)
// [1, "a"]
```

`filter`方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组

```js
[1, 2, 3, 4, 5].filter(function (elem, index, arr) {
  return index % 2 === 0;
});
// [1, 3, 5]
```

`filter`也可以接受第二个参数，用来绑定函数内部的`this`变量

```js
var obj = { MAX: 3 };
var myFilter = function (item) {
  if (item > this.MAX) return true;
};
var arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, obj) // [8, 4, 9]
```

##### 3.13 some()，every()

均返回布尔值，判断成员是否符合某种条件。它们接受一个函数作为参数，所有成员依次执行该函数，该函数接受三个参数：当前成员、当前位置和整个数组，然后返回布尔值

`some`方法是只要一个成员的返回值是`true`，则整个`some`方法的返回值就是`true`

```js
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
// true
//如果数组arr有一个成员大于等于3，some方法就返回true
```

`every`方法是所有成员的返回值都是`true`，整个`every`方法才返回`true`

```js
var arr = [1, 2, 3, 4, 5];
arr.every(function (elem, index, arr) {
  return elem >= 3;
});
// false
//数组arr并非所有成员大于等于3，所以返回false
```

注意，对于空数组，`some`方法返回`false`，`every`方法返回`true`，回调函数都不会执行

```js
function isEven(x) { return x % 2 === 0 }
[].some(isEven) // false
[].every(isEven) // true
```

`some`和`every`方法还可以接受第二个参数，用来绑定参数函数内部的`this`变量 

##### 3.14 reduce()，reduceRight()

`reduce`方法和`reduceRight`方法依次处理数组的每个成员，最终累计为一个值。（前者从左到右处理，后者从右到左处理）其他完全一样

`reduce`方法和`reduceRight`方法的第一个参数都是一个函数。该函数接受以下四个参数。

1. 累积变量，默认为数组的第一个成员（必须）
2. 当前变量，默认为数组的第二个成员（必须）
3. 当前位置（从0开始）（可选）
4. 原数组（可选）

```js
[1, 2, 3, 4, 5].reduce(function (a, b) {
  console.log(a, b);
  return a + b;
})
// 1 2
// 3 3
// 6 4
// 10 5
//最后结果：15
//a为上一次的返回值
```

第一次执行，`a`是数组的第一个成员`1`，`b`是数组的第二个成员`2`。第二次执行，`a`为上一轮的返回值`3`，`b`为第三个成员`3`。第三次执行，`a`为上一轮的返回值`6`，`b`为第四个成员`4`。第四次执行，`a`为上一轮返回值`10`，`b`为第五个成员`5`。至此所有成员遍历完成，整个方法的返回值就是最后一轮的返回值`15`。

若要对累积变量指定初始值，可放在第二个参数

```js
[1, 2, 3, 4, 5].reduce(function (a, b) {
  return a + b;
}, 10);
// 25
```

例子（还可把函数拿出来）

```js
function subtract(prev, cur) {
  return prev - cur;
}
[3, 2, 1].reduce(subtract) // 0
[3, 2, 1].reduceRight(subtract) // -4
```

##### 3.15 indexOf()，lastIndexOf()

`indexOf`返回指定元素在数组中第一次出现的位置，如果没有出现则返回`-1`。还可接受第二个参数，表示搜索开始的位置

```js
['a', 'b', 'c'].indexOf('a', 1) // -1
```

`lastIndexOf`返回指定元素在数组中最后一次出现的位置，如果没有出现则返回`-1`

注意：由于上述两个方法内部采用的是严格相等`===`运算，所以不能用来搜寻`NaN`

```js
[NaN].indexOf(NaN) // -1
[NaN].lastIndexOf(NaN) // -1
```

##### 3.16 链式使用

上述数组方法中，有些返回的仍是数组，所以可以链式引用。例：

```js
var users = [
  {name: 'tom', email: 'tom@example.com'},
  {name: 'peter', email: 'peter@example.com'}
];
users
.map(function (user) {
  return user.email;
})
.filter(function (email) {
  return /^t/.test(email);
})
.forEach(function (email) {
  console.log(email);
});
// "tom@example.com"
```

上面代码中，先产生一个所有 Email 地址组成的数组，然后再过滤出以`t`开头的 Email 地址，最后将它打印出来