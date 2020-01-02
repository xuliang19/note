##### 1. 返回值

`return`只能返回一个值，要返回多个值，可用JSON

##### 2. argument

参数个数不定，`argument`是一个类数组，每个参数对应`argument`里的成员

```js
//求任意个数数字之和
function sum() {
    var result = 0;
    for (var i = 0; i < argument.length; i++) {
        result += arguments[i];
    }
    return result;
}
console.log(sum(1, 3, 4))
//这里argument[0] = 1;
```

为了避免在函数里频繁使用`argument`，可以传入参数

```js
function css() {
	return argument[0];
}
//传入参数;等效于
function css(name) {
    return name;
}
```

##### 3. 取非行间样式

`style`只能获取行间样式，想获取非行间样式IE用`currentStyle`，其他浏览器`getComputedStyle()`（综合各种规则后最终的结果）

```js
var oDiv = document.getElementById('div1');
//下面不会有显示
console.log(oDiv.style.width);
//下面会显示;浏览器兼容性写法
if (oDiv.currentStyle) {
    //IE8
    console.log(oDiv.currentStyle.width);
}
else {
    //FF
    console.log(getComputedStyle(oDiv).width);
}
//可以将上述封装在一个函数里
funciton getStyle(obj, name) {
    if (obj.currentStyle) {
        //IE8
        console.log(obj.currentStyle.name);
    }
    else {
        //FF
        console.log(getComputedStyle(obj).name);
    }
}
```

上面这种`if...else`兼容性写法很常用。`getComputedStyle`还可接受第二个参数，表示当前元素的伪元素例如

```js
var result = window.getComputedStyle(div, ':before');
```

?> 注意：无法读取缩写属性，例如`font`，`background`，想读取缩写，只能将对应全程写出，例如`fontSize`

##### 3. 数组

###### 3.1 数组属性

`length`既可以获取，又可以设置（可以通过这个快速清空数组）

###### 3.2 常用的数组方法

添加

- `push(元素)`，从尾部添加（用的比较多）
- `unshift(元素)`，从头部添加

删除

- `pop()`，从尾部删除
- `shift()`，从头部删除

想从中间删除，插入，替换

- `splice(start, count[, addElement, ...])`，在指定位置删除指定个数元素（还可以在指定位置添加元素）

  ```js
  var arr = [1, 2, 3, 4, 5, 6];
  //删除
  arr.splice(2, 3);//返回删除的数组[3, 4, 5]
  console.log(arr);//[1, 2, 6]
  //插入元素
  arr.splice(2, 0, 'a', 'b', 'c');
  //替换
  arr.splice(2, 2, 'a', 'b');
  ```

连接数组

- `arr1.concat(arr2);`

分隔符

- 数组使用`arr1.join('-')`，返回字符串
- 字符串使用`string1.split('-')`，返回一个数组（相当于互逆操作）

排序

- `arr1.sort()`，类似文件夹中的按名称排序，对于数字排序会有问题（会把数字转为字符串，比较字典值，直接就从第一个数字开始一个个对比，例如`10`排在`5`前面）。解决这个问题可以传入一个函数作为参数

  ```js
  arr.sort(function(n1, n2) {
      return n1 - n2;
  });
  ```