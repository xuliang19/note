#### 1. 静态属性

`Math`对象的静态属性，提供这些数学常数

|      属性      |          常数          |
| :------------: | :--------------------: |
|    `Math.E`    |           e            |
|   `Math.LN2`   |          Ln2           |
|  `Math.LN10`   |          Ln10          |
|  `Math.LOG2E`  | log2 |
| `Math.LOG10E`  | log10 |
|   `Math.PI`    |           π            |
| `Math.SQRT1_2` |    √0.5    |
|  `Math.SQRT2`  |     √2     |

这些方法均只读，不可修改

#### 2. 静态方法

|      方法       |   作用   |
| :-------------: | :------: |
|  `Math.abs()`   |  绝对值  |
|  `Math.ceil()`  | 向上取整 |
| `Math.floor()`  | 向下取整 |
|  `Math.max()`   |  最大值  |
|  `Math.min()`   |  最小值  |
|  `Math.pow()`   | 指数运算 |
|  `Math.sqrt()`  |  平方根  |
|  `Math.log()`   | 自然对数 |
|  `Math.exp()`   | e的指数  |
| `Math.round()`  | 四舍五入 |
| `Math.random()` |  随机数  |

##### 2.1 Math.abs()

```js
Math.abs(-1) // 1
```

##### 2.2 Math.max()，Math.min()

```js
Math.max(2, -1, 5) // 5
Math.min(2, -1, 5) // -1
Math.min() // Infinity
Math.max() // -Infinity
//参数为空则返回Infinity
```

##### 2.3 Math.floor()，Math.ceil()

`Math.floor`向下取整（取小于参数的最大整数）

```Js
Math.floor(3.2) // 3
Math.floor(-3.2) // -4
```

`Math.ceil`向上取整（取大于参数的最小整数）

```js
Math.ceil(3.2) // 4
Math.ceil(-3.2) // -3
```

两个方法结合使用，实现总是返回数值的整数部分函数

```js
function ToInteger(x) {
  x = Number(x);
  return x < 0 ? Math.ceil(x) : Math.floor(x);
}
ToInteger(3.2) // 3
ToInteger(3.5) // 3
```

##### 2.4 Math.round()

四舍五入

```js
Math.round(0.5) // 1
Math.round(0.6) // 1
// 等同于
Math.floor(x + 0.5)
```

注意，它对负数处理（`0.5`的部分）

```js
Math.round(-1.5) // -1
Math.round(-1.6) // -2
```

##### 2.5 Math.pow()

第一个参数为底，第二个为幂

```js
// 等同于 2 ** 3
Math.pow(2, 3) // 8
```

##### 2.6 Math.sqrt()

负数的平方根返回`NaN`

```js
Math.sqrt(4) // 2
Math.sqrt(-4) // NaN
```

##### 2.7 Math.log()

`Math.log`方法返回以`e`为底的自然对数值

```js
Math.log(Math.E) // 1
```

##### 2.8 Math.exp()

`Math.exp`方法返回常数`e`的参数次方。

```js
Math.exp(3) // 20.085536923187668
```

##### 2.9 Math.random()

`Math.random`返回`[0, 1)`之间的一个伪随机数

```js
Math.random() // 0.7151307314634323
```

```js
function getRandomArbitrary(min, max) {
  return Math.random() * (max - min) + min;
}
getRandomArbitrary(1.5, 6.5)
// 2.4942810038223864
```

任意范围的随机整数生成函数如下。

```js
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
getRandomInt(1, 6) // 5
```

返回随机字符的例子如下。

```js
function random_str(length) {
  var ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
  ALPHABET += 'abcdefghijklmnopqrstuvwxyz';
  ALPHABET += '0123456789-_';
  var str = '';
  for (var i = 0; i < length; ++i) {
    var rand = Math.floor(Math.random() * ALPHABET.length);
    str += ALPHABET.substring(rand, rand + 1);
  }
  return str;
}
random_str(6) // "NdQKOr"
```

##### 2.10 三角函数方法

|     方法      |  作用  |
| :-----------: | :----: |
| `Math.sin()`  |  sin   |
| `Math.cos()`  |  cos   |
| `Math.tan()`  |  tan   |
| `Math.asin()` | arcsin |
| `Math.acos()` | arccos |
| `Math.atan()` | arctan |