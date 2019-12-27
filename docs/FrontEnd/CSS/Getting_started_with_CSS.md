CSS (Cascading Style Sheets)用来样式化网页

#### 1. 语法

由选择器（selector）开头(`h1`)，接着`{}`，`{}`内部为`property: value;`形式的规则，用于设置元素属性值。整个称为声明（eclaration）。CSS通常包含多个声明，声明内部可以由多个规则

```css
h1 {
    color: red;
    font-size: 5em;
}
p {
    color: black;
}
```

#### 2. 添加CSS

添加CSS方式有三种，优先级：内联>内部>外部。工程中为了内容样式分离，通常使用**外部**的方法添加。前两种通常用于代码测试（因为比较难维护）

```html
/* 内联 */
<h1 style="color: blue;background-color: yellow;">Hello World!</h1>

/* 内部 */
<head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
	<style>
		h1 {
			color: blue;
		}
	</style>
</head>

/* 外部，type可以不加 */
<link rel="stylesheet" href="styles.css">
```

#### 3. 属性和值

- 属性：标识符表示你想要改变的样式特征（例如`font-size`,`width`）

- 值：通常是由数值或关键字，也有可能是函数（例如`calc()`）

  ```css
  div {
      width: calc(90% - 30px);
  }
  ```

属性中包含多个值用空格分开

```css
margin: 0 auto;
```

浏览器遇到无法解析的属性时会跳过此属性。遇到无法解析的选择器时，内部的所有属性会跳过

#### 4. @规则

用的比较多的`@rulues`是`@media`，用来表示当处于某一条件时候样式改变。`@import`用来导入某个模块，可能是字体，样式等等

```css
@media (min-width: 30em) {
    body {
        background-color: blue;
    }
```

```css
@import 'style2.css'
```

#### 5. 简写

简写（shorthands）在一行中设置多个属性值，使代码更整洁，如`font`,`background`,`padding`,`margin`,`border`等等

```css
/* padding: top/bottom,then left/right */
padding: 10px 15px 15px 5px;

/* 相当于 */
padding-top: 10px;
padding-right: 15px;
padding-bottom: 15px;
padding-left: 5px;
```

```css
background: red url(bg-graphic.png) 10px 10px repeat-x fixed;

/* 相当于 */
background-color: red;
background-image: url(bg-graphic.png);
background-position: 10px 10px;
background-repeat: repeat-x;
background-scroll: fixed;
```

#### 6. 注释

`/* comment */`