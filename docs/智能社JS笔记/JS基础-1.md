ECMA：几乎没有不兼容

DOM：有一些操作不兼容

BOM：用的少

1. 这种方式一般用的很少，一般`href`是空着

   ```html
   <a href="javascript:alert("a");">链接</a>
   ```

2. HTML怎么写，JS里就怎么写（除`className`之外，`class`是关键字）

   ```js
   //添加属性
   var link = document.getElementById("l1");
link.href = "some code"
   ```

3. 函数传参，当有一部分东西定不下来时候

   ```js
   function setCol(color) {
       div.style.background = color;
   }
   <button onclick="setCol("red")">变红</button>
   <button onclick="setCol("green")">变绿</button>
   ```
4. 用`style["property"]`来获取属性，这样就可以写入变量了。JS中所有`.`都可以用`[]`代替，例如`document["getElementById"]("id")`

   ```js
   //这样不对，会失效,JS无法识别.name，误以为新属性
   function setCol(name, value) {
       div.style.name = value;
   }
   //一般情况用点，这时候应该用[]
   function setCol(name, value) {
       div.style.[name] = value;
   }
   <button onclick="setCol("width", "400px")">变宽</button>
   <button onclick="setCol("color", "green")">变绿</button>
   ```

5. `style`和`className`最好别混用，`style`行间优先级大于内嵌，同一个样式，先使用`style`再使用`className`会失效（推荐用`className`）

6. 一般将`script`放在`body`最底部（防止元素未加载无法读取），放在`head`里也可以。放在`window.onload`里面

   ```js
   window.onload = function() {
   //some code
   }
   ```

7. 全选，全不选，反选功能

   ```html
   <input id="btn1" type="button" value="全选">
   <input id="btn2" type="button" value="全不选">
   <input id="btn3" type="button" value="反选">
   <div id="div1">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   	<input type="checkbox">
   </div>
   ```

   ```js
   var myDiv = document.getElementById("div1");
   var myCh = myDiv.getElementsByTagName("input");
   var btn1 = document.getElementById("btn1"),
   	btn2 = document.getElementById("btn2"),
   	btn3 = document.getElementById("btn3");
   
   btn1.onclick = function() {
   	for (var i = 0; i < myCh.length; i++) {
   		myCh[i].checked = true;
   	}
   }
   btn2.onclick = function() {
   	for (var i = 0; i < myCh.length; i++) {
   		myCh[i].checked = false;
   	}
   }
   btn3.onclick = function() {
   	for (var i = 0; i < myCh.length; i++) {
   		if (myCh[i].checked === true) {
   			myCh[i].checked = false;
   		}else {
   			myCh[i].checked = true;
   		}
   	}
   }
   ```

8. 有一个通用的思想就是，先隐藏所有的，再点亮某一个，执行前先重置（轮播图也有个这思想）

   选项卡实现（注意里面的思想）

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
   	<meta charset="UTF-8">
   	<meta name="viewport" content="width=device-width, initial-scale=1.0">
   	<meta http-equiv="X-UA-Compatible" content="ie=edge">
   	<title>Document</title>
   	<style>
   		#container div {
   			width: 200px;
   			height: 200px;
   			background: grey;
   			display: none;
   		}
   		/* 这里注意css权重，要加#container，不然无效 */
   		#container .show {
   			display: block;
   		}
   		/* 这里最好加上#container */
   		#container .active {
   			background: yellow;
   		}
   	</style>
   </head>
   <body>
   	<div id="container">
   		<input type="button" value="国庆">
   		<input type="button" value="中秋">
   		<input type="button" value="五一">
   		<input type="button" value="春节">
   		<div class="show">1111</div>
   		<div>2222</div>
   		<div>3333</div>
   		<div>4444</div>
   	</div>
   	<script>
   		// 先获取这个块，在块上选择，保证不会误取到其他元素
   		var myCon = document.getElementById("container");
   		var myIpt = myCon.getElementsByTagName("input");
   		var myDiv = myCon.getElementsByTagName("div");
   		// 首先绑定事件
   		for (var i = 0; i < myIpt.length; i++) {
   			// 在button上加index，每一个就对应div的下标
   			// index不能直接放HTML里，属于自定义属性
   			myIpt[i].index = i;
   			// 绑定事件
   			myIpt[i].onclick = function() {
   				for (var i = 0; i < myIpt.length; i++) {
   					// 清除所有button激活背景
   					myIpt[i].className = "";
   					// 清除所有display:block
   					myDiv[i].className = "";
   				}
   				// 这里this事件发生的元素，如果是代理的话，要用event.target
   				this.className = "active";
   				
   				myDiv[this.index].className = "show";
   			}
   		}
   	</script>
   </body>
   </html>
   ```

9. 需要制作一个日历时候，上述就不太管用了（`div`太多），对于修改文字内容的用`innerHTML`。`innerHTML`内容可以设置为HTML，例：

   ```js
   div.innerHTML = "<b>哈哈哈</b>"
   ```

   制作日历时候需要用到数组，