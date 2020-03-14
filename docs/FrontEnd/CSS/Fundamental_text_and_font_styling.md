样式化文本可分为两类，**字体样式**和**文本布局**

#### 1. 字体样式

##### 1.1 颜色

`color`属性设置选中的元素前景色内容，通常指文本

```css
p {
    color: red;
}
```

##### 1.2 字体种类

`font-family`允许指定一个字体或一个字体列表

```css
p {
    font-family: Arial;
}
```

###### 网页安全字体

网页安全字体在所有常用的操作系统中都可用

|    字体名称     |    泛型    | 注意                                                         |
| :-------------: | :--------: | ------------------------------------------------------------ |
|      Arial      | sans-serif | 通常认为最佳做法还是添加 Helvetica 作为 Arial 的首选替代品，尽管它们的字体面几乎相同，但 Helvetica 被认为具有更好的形状，即使Arial更广泛地可用。 |
|   Courier New   | monospace  | 某些操作系统有一个 Courier New 字体的替代（可能较旧的）版本叫Courier。使用Courier New作为Courier的首选替代方案，被认为是最佳做法。 |
|     Georgia     |   serif    |                                                              |
| Times New Roman |   serif    | 某些操作系统有一个 Times New Roman 字体的替代（可能较旧的）版本叫 Times。使用Times作为Times New Roman的首选替代方案，被认为是最佳做法。 |
|  Trebuchet MS   | sans-serif | 您应该小心使用这种字体——它在移动操作系统上并不广泛。         |
|     Verdana     | sans-serif |                                                              |

###### 默认字体

CSS定义了5个常用的默认字体名称：`serif`, `sans-serif`, `monospace`, `cursive`和 `fantasy`。前三个比较兼容，后面两个要多注意

###### 字体栈

为了保证网页字体的可用性，最好提供一个字体栈（font stack），这样当第一个字体不能用时，浏览器会选择第二个字体

```css
p {
  font-family: "Trebuchet MS", Verdana, sans-serif;
}
```

?> 有些字体名称不止一个单词，这时就要用引号

##### 1.3 字体大小

`font-size`常用单位是`px`，`em`，`rem`(不支持IE8及以下版本)

通常为了避免出现无穷小数的情况，我们可以将`<html>`字体设置为`10px`，这样计算起来就很方便了

```css
html {
  font-size: 10px;
}
h1 {
  font-size: 2.6rem;
}
p {
  font-size: 1.4rem;
  color: red;
  font-family: Helvetica, Arial, sans-serif;
}
```

##### 1.4 字体常见的几种属性

几种常用的字体样式

- `font-style`：用来打开和关闭字体斜体

  - `normal`：将文字设为普通字体（将存在的斜体关闭）
  - `italic`：将文字设为斜体；如果斜体版本不可用，那么会用oblique状态模拟italic
  - `oblique`：将字体设置为斜体字体的模拟版本

- `font-weight`：设置字体粗细

  - `normal`：普通
  - `bold`：加粗
  - `lighter`：设为比父元素更细的字体
  - `bolder`：设为比父元素更粗的字体
  - `100`—`900`：使用数字控制字体粗细

- `text-transform`：转换字体

  - `none`：防止任何转型
  - `uppercase`：转为大写
  - `lowercase`：转为小写
  - `capitalize`：单词首字母大写
  - `full-width`：将所有字体转为全角（固定宽度的正方形），可以使拉丁字符和亚洲语言（中文，日文，韩文）对齐

- `text-decoration`：设置/取消文本装饰

  - `none`：取消已经存在的任何文本装饰
  - `underline`：下划线
  - `overline`：上划线
  - `line-through`：划除线

  `text-decoration`是一个缩写形式，它由 [`text-decoration-line`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-line), [`text-decoration-style`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-style) 和 [`text-decoration-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-color) 构成

##### 1.5 文字阴影

`text-shadow`为文字添加阴影，和`box-shadow`有点类似，也是四个值，分别代表水平偏移，垂直偏移，模糊半径，颜色

```css
p {
    text-shadow: 4px 4px 5px red;
}
```

同样可以使用多种阴影，用`,`分隔开

```css
text-shadow: -1px -1px 1px #aaa,
             0px 4px 1px rgba(0,0,0,0.5),
             4px 4px 5px rgba(0,0,0,0.7),
             0px 0px 7px rgba(0,0,0,0.4);
```

#### 2. 文本布局

##### 2.1 文本对齐

`text-align`用来控制文本的在content中的对齐方式

- `left`：左对齐
- `right`：右对齐
- `center`：居中
- `justify`：两端对齐，会改变单词之间的间距

##### 2.2 行高

`line-height`设置文本每行之间的高，除了设置常用单位之外，还可以设置无单位值，代表乘数，乘以当前文字的`font-size`大小

```css
p {
	line-height: 1.5;
}
```

##### 2.3 字母和单词间距

`letter-spacing`设置字母之间的间距

`word-spacing`设置单词之间的间距

```css
p::first-line {
	letter-spacing: 2px;
	word-spacing: 4px;
}
```

##### 2.4 文本垂直对齐

[`vertical-align`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式

#### 3. 其他属性

Font 样式:

- [`font-variant`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant): 在小型大写字母和普通文本选项之间切换。
- [`font-kerning`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-kerning): 开启或关闭字体间距选项。
- [`font-feature-settings`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-feature-settings): 开启或关闭不同的 [OpenType](https://en.wikipedia.org/wiki/OpenType) 字体特性。
-  [`font-variant-alternates`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-alternates) : 控制给定的自定义字体的替代字形的使用。
- [`font-variant-caps`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant-caps): 控制大写字母替代字形的使用。
-  [`font-variant-east-asian`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-east-asian) : 控制东亚文字替代字形的使用, 像日语和汉语。
-  [`font-variant-ligatures`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-ligatures) : 控制文本中使用的连写和上下文形式。
-  [`font-variant-numeric`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric) : 控制数字，分式和序标的替代字形的使用。
-  [`font-variant-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-position) : 控制位于上标或下标处，字号更小的替代字形的使用。
- [`font-size-adjust`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-size-adjust): 独立于字体的实际大小尺寸，调整其可视大小尺寸。
- [`font-stretch`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-stretch): 在给定字体的可选拉伸版本中切换。
- [`text-underline-position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-underline-position): 指定下划线的排版位置，通过使用 `text-decoration-line` 属性的`underline` 值。
- [`text-rendering`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-rendering): 尝试执行一些文本渲染优化。

文本布局样式：

- [`text-indent`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-indent): 指定文本内容的第一行前面应该留出多少的水平空间。
- [`text-overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow): 定义如何向用户表示存在被隐藏的溢出内容。
- [`white-space`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space): 定义如何处理元素内部的空白和换行。
- [`word-break`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-break): 指定是否能在单词内部换行。
- [`direction`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/direction): 定义文本的方向 (这取决于语言，并且通常最好让HTML来处理这部分，因为它是和文本内容相关联的。)
- [`hyphens`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/hyphens): 为支持的语言开启或关闭连字符。
- [`line-break`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-break): 对东亚语言采用更强或更弱的换行规则。
- [`text-align-last`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align-last): 定义一个块或行的最后一行，恰好位于一个强制换行前时，如何对齐。
-  [`text-orientation`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-orientation) : 定义行内文本的方向。
- [`word-wrap`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-wrap): 指定浏览器是否可以在单词内换行以避免超出范围。
- [`writing-mode`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/writing-mode): 定义文本行布局为水平还是垂直，以及后继文本流的方向。

#### 4. Font简写

`font`简写顺序如下：`font-style`, `font-variant`, `font-weight`, `font-stretch`, `font-size`, `line-height`, `font-family`

注意：若使用简写属性，`font-size`和`font-family`一定要指定；`font-size`和`line-height`之间必须放一个正斜杠`/`

```css
p {
    font: italic normal bold normal 3em/1.5 Helvetica, Arial, sans-serif;
}
```

#### 5. Web字体

当我们需要一些特殊字体时候，可以用CSS`@font-face`模块，它指定要下载的字体。例子如下：

```css
@font-face {
    font-family: "myFont";
    src: url("myFont.ttf");
}
```

然后在设置字体时候，就可以像使用本地字体一样了

```css
html {
	font-family: "myFont";
}
```

一些免费的字体网站：[Font Squirre](https://www.fontsquirrel.com/)，[dafont](http://www.dafont.com/) 和 [Everything Fonts](https://everythingfonts.com/)

代码在线生成：[Webfont Generator](https://www.fontsquirrel.com/tools/webfont-generator) 