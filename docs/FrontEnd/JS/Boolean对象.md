#### 1. 概述

`Boolean`对象是布尔值对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用

注意，`false`对应的包装对象实例，布尔运算结果也是`true`（因为已经是一个对象了，对象的布尔值均为`true`）

```js
var b = new Boolean(true);
typeof b // "object"
b.valueOf() // true

if (new Boolean(false)) {
  console.log('true');
} // true
if (new Boolean(false).valueOf()) {
  console.log('true');
} // 无输出
```

#### 2. Boolean 函数的类型转换作用

`Boolean`对象除了用作构造函数，还可以单独使用，将任意值转为布尔值

```js
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean('') // false
Boolean(NaN) // false
Boolean(1) // true
Boolean('false') // true
Boolean([]) // true
Boolean({}) // true
Boolean(function () {}) // true
Boolean(/foo/) // true
```

使用双重否运算`!`也可将任意值转为布尔值

```js
!!undefined // false
!!null // false
!!0 // false
!!'' // false
!!NaN // false
!!1 // true
!!'false' // true
!![] // true
!!{} // true
!!function(){} // true
!!/foo/ // true
```

