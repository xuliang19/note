#### 广义的定义

JavaScript是一种脚本，一门编程语言。可以在网页上实现复杂的功能，网页不再是简单的静态信息。

#### 它可以做什么

- 在变量中存储有用的值
- 操作一段文本
- 运行代码响应网页中发生的特定事件
- 等等

还有一个重要的功能就是API，API通常分为**浏览器API**和**第三方API**：

浏览器API（默认嵌入在浏览器中的）：

- `文档对象模型API（DOM（Document Object Model）API）`实现创建、移除和修改HTML，一些小弹窗等
-  `地理位置 API（Geolocation API）`获取地理信息
- `画布（Canvas）` 和 `WebGL API` 可以创建生动的 2D 和 3D 图像
- 等等

第三方API（一般需要从网上获取它们的信息和代码），例如：

- `Twitter API` 和 `新浪微博API` 可以在网站展示最新的推文
- `谷歌地图API`和`高德地图API` 可以在网站嵌入定制的地图等等

#### JS运行次序

执行代码时候通常按照从上到下执行代码，所以在引用对象时候确保它已经存在

#### 一些术语

解释代码和编译代码：

- 解释型语言中，代码自上而下运行，且实时返回结果，代码在浏览器执行前无需转化为其它形式
- 编译代码需要转化成另一种形式才得以运行，比如C++需要先转化为汇编语言

服务端代码和客户端代码：

- 客户端代码就是在用户电脑上运行的代码，浏览网页时客户端代码会被下载，由浏览器运行展示
- 服务器代码在服务器上运行，运行结果由浏览器下载展示。流行的服务器端web语言包括：PHP、Python、Ruby、ASP.NET以及JS（JS也可以用作服务器端语言，比如现在流行的Node.js环境）

#### 添加JS

- 内部JS

  在`</body>`前添加`<script></script>`，这样可以确保页面加载完成后再加载JS，`type`属性默认值为`text/javascript`，所以可以不写`type`
  
  > 在解释器对`<script>`元素内部的所有代码求值完毕以前，页面中的其余内容都不会被浏览器加载或显示   

- 外部JS

  ```html
  <script src="script.js"></script>
  ```

  如果JS文件很大，我们不能再使用把`script`放在底部的办法，因为JS要等所有HTML加载完后才开始运行，对性能影响较大。这时候可以使用`async`或`defer`解决（有`async`或`defer`则可以放在文档开头，否则放在底部）

  `async`：只适用于外部脚本，告诉浏览器立即下载脚本，但是脚本可能不会按照标记的顺序执行，例如：

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<title>Example HTML Page</title>
  		<script type="text/javascript" async src="example1.js"></script>
  		<script type="text/javascript" async src="example2.js"></script>
  	</head>
  	<body>
  		<!-- 这里放内容 -->
  	</body>
  </html>
  ```

  第二个脚本可能会在第一个之前运行，`async`目的是为了不让页面等待脚本下载和执行，从而异步加载页面其他内容，建议异步脚本不要在加载期间修改DOM。（异步脚本一定会在页面load事件前执行，但不一定在DOMContentLoaded前执行）

  `defer`

  注意：外部代码中如果含有`</script>`字符则需要转义为`<\/script>`

  > 当在外部脚本中嵌入代码时候，代码会被忽略，也就是外部优先级高于内部
  >
  > ```html
  > <script src="script.js">alert("hello")</script>//中间代码无效
  > ```

  