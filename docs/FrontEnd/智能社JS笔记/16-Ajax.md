#### 1. Ajax基础

- 请求并显示静态TXT文件
  - 字符集编码都用utf-8（文本也是）

  - 阻止缓存——有很多方法，例如：利用不同域名

    ```js
    //在网址后面加日期,毫秒计数
    ajax('aaa.txt?t =' + new Date().getTime(), function(str) {}, function() {});
    ```

- 请求动态数据（Json）

  - 使用eval读取字符串中的数组

    ```js
    var arr = eval(str);
    ```


- HTTP请求方法
  - GET——用于获取数据（如：浏览帖子）
  - POST——用于上传数据（如：用户注册）
  - 两者区别
    - get将数据放入url，通过网址传递（容量小，有缓存，安全性差）
    - post通过http content传递（无缓存）

#### 2.创建Ajax

同步：事情一件一件来。异步：多个事情一起。Ajax都是异步

`oAjax.readyState`的值

- 0——（未初始化）还没有调用`open()`方法
- 1——（载入）已调用`send()`方法，正在发送请求
- 2——（载入完成）`send()`方法完成，已收到全部响应内容
- 3——（解析）正在解析响应内容
- 4——（完成）响应内容解析完成（无论调用成功或失败）

```js
var oBtn = document.getElementById("btn1");
oBtn.onclick = function() {
    ajax('Ajax_test.txt', function(str) {
        console.log(str);
    }, function() {})
};
function ajax(url, fnSucc, fnFail) {
    //1.创建Ajax对象
    // 这里有个兼容IE6的判断,我没写
    var oAjax = new XMLHttpRequest();
    // 2.连接服务器
    // open(方法, 文件名, 异步传输)默认异步
    oAjax.open('GET', url, true);
    // 3.发送请求
    oAjax.send();
    // 4.接收返回
    oAjax.onreadystatechange = function () {
        // oAjax.readyState表示浏览器和服务器进行到哪一步了
        if (oAjax.readyState === 4) {
            if (oAjax.status === 200) {
                fnSucc(oAjax.responseText);
            }
            else {
                // 判断是否传入fnFail
                if (fnFail) {
                    fnFail(oAjax.status);
                }
            }
        }
    }
}
```

