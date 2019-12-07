#### JS动态修改CSS样式方法

两大类，一种是在元素内新建`style`内联属性覆盖其他样式，缺点很明显，例如还会导致伪类失效；另一种是增加样式引用，这种比较好

- 第一大类两种方法

  ```javascript
  var obj = doucument.getElementById("id");
  //方法一(注意大小写)
  obj.style.backgroundColor = "blue";
  //方法二(注意大小写)
  obj.style.cssText = "background-color:blue";
  //这里为了避免覆盖之前的样式,可以用累加
  obj.style.cssText += "background-color:blue";
  ```

- 第二大类两种方法

  ```javascript
  var obj = doucument.getElementById("id");
  //方法一，事先设置好样式，也可以放外部.style2 {...}
  obj.setAttribute("class", "style2")
  //用className,用累加可以不覆盖之前的样式
  obj.className += " ";//要用空格分开属性。就是直接设置里面的字符
  obj.className += "style2"
  //方法二，外部设置好样式（这里也可以通过属性累加）
  var obj = 获取<link>元素； 
  obj.setAttribute("href", "样式路径")
  //setAttribute是有就覆盖，没有就创建
  ```
