#### 1.cookie基础

页面用来保存信息——比如自动登录，记住用户名

##### cookie特性

- 整个网站共享一个cookie
- 数量、大小有限
- 过期时间

```js
//在cookie '=' 表示添加
document.cookie = 'user=blue';
document.cookie = 'pass=123456';
```

如果不指定过期时间，浏览器关闭就cookie就清除了。所以要设置过期时间

```js
var oDate = new Date();
oDate.setDate(oDate.getDate() + 14);//指定14天后过期
document.cookie = 'user=blue;expires=' + oDate;
```

##### 封装cookie函数

```js
function setCookie(name, value, iDay) {
    var oDate = new Date();
    oDate.setDate(oDate.getDate() + iDay);
    document.cookie = name + '=' + value + ';expires=' + oDate;
}
```

##### 读取cookie

cookie存储格式如下

```
user=blue; pass=1234; c=22; 
```

读取cookie数据可使用`split`

```js
function getCookie(name) {
    var arr = document.cookie.split('; ');
    for (var i = 0; i < arr.length; i++) {
        var arr2 = arr[i].split('=');
        if (arr2[0] === name) {
            return arr2[1];
        }
    }
    return '';
}
```

##### 删除cookie

```js
function removeCookie(name) {
    // 将过期时间设为-1天
    setCookie(name, 1, -1);
}
```

