#### 概述

`String`对象是字符串对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用

字符串对象是一个类似数组的对象（`Number`和`Boolean`都没有这个特点，直接是一个整体）

```js
new String('abc')
// String {0: "a", 1: "b", 2: "c", length: 3}
(new String('abc'))[1] // "b"
```

上述`abc`的字符串对象有数值键（`0`、`1`、`2`）和`length`属性，所以可以像数组一样取值

除了用作构造函数，`String`还可当做工具方法将任意类型值转为字符串

```js
String(true) // "true"
String(5) // "5"
```

#### 静态方法

##### String.fromCharCode()

`String.fromCharCode()` 参数是一个或多个代表Unicode码点的数值，返回值是这些码点组成的字符串

```js
String.fromCharCode() // ""
String.fromCharCode(97) // "a"
String.fromCharCode(104, 101, 108, 108, 111)
// "hello"
```

更多信息参考[JS入门](https://wangdoc.com/javascript/stdlib/string.html)

#### 实例属性

##### String.prototype.length

返回字符串的长度

```js
'abc'.length // 3
```

#### 实例方法

##### String.prototype.charAt()

`charAt`返回指定位置的字符

```js
var s = new String('abc');
s.charAt(1) // "b"
s.charAt(s.length - 1) // "c"
```

这个方法完全可以用数组下标替代

```js
'abc'.charAt(1) // "b"
'abc'[1] // "b"
```

若参数为负数或者长度大于等于字符串长度，`charAt`返回空字符串

##### String.prototype.charCodeAt()

`charCodeAt`返回字符串指定位置的 Unicode 码点（十进制表示）

```js
'abc'.charCodeAt(1) // 98
```

 若为负数，或大于等于字符串长度，`charCodeAt`返回`NaN`

```js
'abc'.charCodeAt(-1) // NaN
```

[参考链接](https://wangdoc.com/javascript/stdlib/string.html#stringprototypelength)

##### String.prototype.concat()

和数组的`concat`类似，用于连接两个字符串，返回一个新字符串（不会改变原字符串）

```js
var s1 = 'abc';
var s2 = 'def';
s1.concat(s2) // "abcdef"
s1 // "abc"
```

可以接受多个参数

```js
'a'.concat('b', 'c') // "abc"
```

如果参数不是字符串，`concat`方法会将其先转为字符串，然后再连接

```js
var one = 1;
var two = 2;
var three = '3';
''.concat(one, two, three) // "123"
one + two + three // "33"
```

注意它和加法运算的区别

##### String.prototype.slice()

和数组方法`slice`类似（不会改变原字符串）

```js
'JavaScript'.slice(0, 4) // "Java"
```

##### String.prototype.substring()

和`slice`方法类似，有这几点不同：

- 第一个参数大于第二个参数，会自动调换位置

  ```js
  'JavaScript'.substring(10, 4) // "Script"
  // 等同于
  'JavaScript'.substring(4, 10) // "Script"
  ```

- 如果是负数，`substring`会将负数转为`0`

  ```js
  'JavaScript'.substring(-3) // "JavaScript"
  'JavaScript'.substring(4, -3) // "Java"
  ```

由于这些规则违反直觉，不建议使用`substring`

##### String.prototype.substr()

和`slice`方法类似，只是它第一个参数是起始位置，第二个是字符串长度。若省略第二个，则到字符串末尾结束

```js
'JavaScript'.substr(4, 6) // "Script"
```

第一个为负数，表示倒数计算字符的位置。第二个为负数将自动转为0，因此会返回空字符串

```js
'JavaScript'.substr(-6) // "Script"
'JavaScript'.substr(4, -1) // ""
```



##### String.prototype.indexOf()，String.prototype.lastIndexOf()

和数组一样

##### String.prototype.trim()

用于去除字符串两端空格、制表符（`\t`、`\v`）、换行符（`\n`）和回车符（`\r`）

```js
'\r\nabc \t'.trim() // 'abc
```

##### String.prototype.toLowerCase()，String.prototype.toUpperCase()

分别将字符串转为小写和大写

##### String.prototype.match()

`match`用于确定字符串中是否含有某个字符串，返回第一个某字符串。没有匹配就返回`null`

```js
'cat, bat, sat, fat'.match('at') // ["at"]
'cat, bat, sat, fat'.match('xt') // null
```

返回的数组有`index`和`input`属性，分别表示匹配字符串开始的位置和原始字符串

```js
var matches = 'cat, bat, sat, fat'.match('at');
matches.index // 1
matches.input // "cat, bat, sat, fat"
```

`match`还可用做正则表达式

##### String.prototype.search()，String.prototype.replace()

`search`和`indexOf`类似，返回匹配的第一个位置，若没有则返回`-1`

```
'cat, bat, sat, fat'.search('at') // 1
```

`replace`用于替换匹配的子字符串，一般只替换第一个匹配（除非使用带有`g`的正则表达式）

```js
'aaa'.replace('a', 'b') // "baa"
```

上述方法均可使用正则表达式作为参数

##### String.prototype.split()

`split`按给定规则分割字符串，返回一个由分割出来的子字符串组成的数组

```js
'a|b|c'.split('|') // ["a", "b", "c"]
'a|b|c'.split('') // ["a", "|", "b", "|", "c"]
```

如果省略参数，则数组中就是原字符串

```js
'a|b|c'.split() // ["a|b|c"]
'a||c'.split('|') // ['a', '', 'c']
'|b|c'.split('|') // ["", "b", "c"]
'a|b|'.split('|') // ["a", "b", ""]
```

`split`还可接受第二个参数，限定返回数组的最大成员数

```js
'a|b|c'.split('|', 1) // ["a"]
'a|b|c'.split('|', 2) // ["a", "b"]
```

`split`可接受正则表达式作为参数

##### String.prototype.localeCompare()

`localeCompare`方法用于比较两个字符串。返回一个整数，若小于0表示前者小于后者；等于0表示两者相等；大于0表示前者大于后者

```js
'apple'.localeCompare('banana') // -1
'apple'.localeCompare('apple') // 0
```

该方法的最大特点，就是会考虑自然语言的顺序

```js
//采用的是Unicode码点比较
'B' > 'a' // false
//考虑自然语言排序情况
'B'.localeCompare('a') // 1
```

`localeCompare`第二个参数可指定所使用的语言（默认英语）