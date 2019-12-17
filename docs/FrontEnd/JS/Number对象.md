#### 1. 概述

`Number`对象是数值对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用

作为构造函数时，它用于生成值为数值的对象；作为工具函数时，它可以将任何类型的值转为数值

```js
var n = new Number(1);
typeof n // "object"

Number(true) // 1
```

#### 2. 静态属性

`Number`对象有以下静态属性

|          静态属性          |            定义            |
| :------------------------: | :------------------------: |
| `Number.POSITIVE_INFINITY` |  正的无限，指向`Infinity`  |
| `Number.NEGATIVE_INFINITY` | 负的无限，指向`-Infinity`  |
|        `Number.NaN`        |   表示非数值，指向`NaN`    |
|     `Number.MIN_VALUE`     |       表示最小的正数       |
|    `-Number.MIN_VALUE`     |     表示最接近0的负数      |
| `Number.MAX_SAFE_INTEGER`  | 表示能够精确表示的最大整数 |
| `Number.MIN_SAFE_INTEGER`  | 表示能够精确表示的最小整数 |

```js
Number.POSITIVE_INFINITY // Infinity
Number.NEGATIVE_INFINITY // -Infinity
Number.NaN // NaN
Number.MAX_VALUE
// 1.7976931348623157e+308
Number.MAX_VALUE < Infinity
// true
Number.MIN_VALUE
// 5e-324
Number.MIN_VALUE > 0
// true
Number.MAX_SAFE_INTEGER // 9007199254740991
Number.MIN_SAFE_INTEGER // -9007199254740991
```

#### 3. 实例方法

`Number`对象有4个实例方法，都跟将数值转换成指定格式有关 

##### 3.1 Number.prototype.toString()

`Number`对象设置了自己的`toString`方法，用来将一个数值转为字符串形式。`toString`还可接受一个参数，表示输出的进制，默认十进制

```js
(10).toString(2) // "1010"
(10).toString(8) // "12"
(10).toString(16) // "a"
```

上面代码中，`10`要放在括号中，表示调用对象属性。如果不加会被解释为小数点。

只要不然引擎混淆小数点和对象点运算符，各种写法都能用;

```js
10..toString(2)
// "1010"

// 其他方法还包括
10 .toString(2) // "1010"
10.0.toString(2) // "1010"
10['toString'](2) // "1010"
//对于小数则可以直接使用小数点
```

`toString`是将十进制数转为其他进制，而`parseInt`可将其他进制转为十进制

##### 3.2 Number.prototype.toFixed()

`toFixed()`将一个数转为指定位数的小数（有效范围0-20位），然后返回为字符串

由于浮点数原因，小数`5`的四舍五入是不确定的，这点注意

```js
10.005.toFixed(2) // "10.01"
(10.055).toFixed(2) // 10.05
(10.005).toFixed(2) // 10.01
```

##### 3.3 Number.prototype.toExponential()

`toExponential`方法用于将一个数转为科学计数法。参数为有效小数点位数（0-20位）

```js
(1234).toExponential()  // "1.234e+3"
(1234).toExponential(1) // "1.2e+3"
```

##### 3.4 Number.prototype.toPrecision()

`Number.prototype.toPrecision()`方法用于将一个数转为指定位数的有效数字。该方法四舍五入时不可靠

```js
(12.34).toPrecision(2) // "12"

(12.35).toPrecision(3) // "12.3"
(12.25).toPrecision(3) // "12.3"
```

##### 3.5 Number.prototype.toLocaleString()

`Number.prototype.toLocaleString()`接受一个地区码作为参数，返回一个字符串，表示当前数字在该地区的当地书写形式（若省略，则由浏览器自行处理）

```js
(123).toLocaleString('zh-Hans-CN-u-nu-hanidec')
// "一二三"
```

还可以接受第二个参数，用于指定用途的返回字符串。对象的`style`属性指定输出样式，默认值是`decimal`，表示输出十进制形式。如果值为`percent`，表示输出百分数。

```js
(123).toLocaleString('zh-Hans-CN', { style: 'percent' })
// "12,300%"
```

如果`style`属性的值为`currency`，则可以搭配`currency`属性，输出指定格式的货币字符串形式

```js
(123).toLocaleString('zh-Hans-CN', { style: 'currency', currency: 'CNY' })
// "￥123.00"
```

#### 4. 自定义方法

与其他对象一样，`Number.prototype`对象上面可以自定义方法，被`Number`的实例继承（数值本身无法自定属性）

```js
Number.prototype.add = function (x) {
  return this + x;
};
8['add'](2) // 10
```

由于`add`方法返回的还是数值，所以可以链式运算

```js
Number.prototype.subtract = function (x) {
  return this - x;
};

(8).add(2).subtract(4)
// 6
```

