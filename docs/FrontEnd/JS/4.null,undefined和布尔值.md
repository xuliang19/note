#### 1. null和undefined

##### 1.1 概述

 `null`与`undefined`都可以表示“没有”，含义非常相似， 将一个变量赋值为`undefined`或`null`，语法效果几乎没区别 

```js
var a = undefined;
// 或者
var a = null;
```

当初的JS设计者认为，`null`被当成一个空对象，`null`自动转为0不容易发现错误（那时候JS不包括错误处理机制），因此他又设计了一个`undefined`。区别如下：`null`表示一个“空”对象，转为数值时为`0`；`undefined`是一个表示“此处无定义”的原始值，转为数值时为`NaN`

```js
Number(null) //0
Number(undefined) //NaN
```

##### 1.2 用法和含义

`null`表示空值，即该处的值现在为空。调用函数时，某个参数未设置任何值，这时就可以传入`null`，表示该参数为空。比如，某个函数接受引擎抛出的错误作为参数，如果运行过程中未出错，那么这个参数就会传入`null`，表示未发生错误

 `undefined`表示“未定义”，下面是返回`undefined`的典型场景

```js
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```

#### 2. 布尔值

布尔值代表“真”和“假”两个状态。“真”用关键字`true`表示，“假”用关键字`false`表示。布尔值只有这两个值 

下列运算符会返回布尔值：

- 前置逻辑运算符： `!` (Not)
- 相等运算符：`===`，`!==`，`==`，`!=`
- 比较运算符：`>`，`>=`，`<`，`<=`

自动转化为布尔值的规则是除了下面转为`false`，其余均为`true`

| 数据类型  | 转为false的值  |
| :-------: | :------------: |
|   数值    |     0和NaN     |
|  字符串   | “”（空字符串） |
|   对象    |      null      |
| undefined |   undefined    |

**注意：空数组`[]`和空对象`{}`对应的布尔值都是`true`**

布尔值通常用于程序流的控制

```js
if ("") {
	console.log("true");
}
//没有输出
```

