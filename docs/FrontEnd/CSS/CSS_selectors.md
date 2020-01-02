#### 1. 简易选择器

##### 1.1 标签选择器

`tagName { }`

##### 1.2 类选择器

`.className { }`

##### 1.3 ID选择器

`#idName { }`

##### 1.4 通用选择器

`* { }`，通常用来替换浏览器默认样式，比如重置所有的`margin`和`padding`

```css
* {
    margin: 0;
    padding: 0;
}
```

#### 2. 属性选择器

可以用来选择具有某个属性的元素，或选择指定属性为某个值的元素

|        选择器         |                   描述                   |
| :-------------------: | :--------------------------------------: |
|     `[attribute]`     |       用于选取带有指定属性的元素。       |
|  `[attribute=value]`  |     用于选取带有指定属性和值的元素。     |
| `[attribute~=value]`  | 用于选取属性值中包含指定**单词**的元素。 |
| `[attribute\|=value]` | 用于选取以指定**单词**开头的属性值的元素 |
| `[attribute^=value]`  |      匹配属性值以指定值开头的元素。      |
| `[attribute$=value]`  |      匹配属性值以指定值结尾的元素。      |
| `[attribute*=value]`  |      匹配属性值中包含指定值的元素。      |

比如我想选择列表中所有的外部链接

```css
ul [href^="http"] {
    /* some code */
}
```

比如我想选择很多`input`中的button

```css
input[type="button"] {
    /* some code */
}
```

那些要求包含某单词的属性选择器通常用来选择`class`。这些值匹配区分大小写，若像让其不区分大小写，可在结尾加`i`

```css
input[type="button" i] {
    /* some code */
}
```

#### 3. 伪类和伪元素

##### 3.1 伪类

伪类选择的是在**特定状态下的元素**（比如`hover`，`active`等），就好像给元素添加类

```css
/* 当鼠标移到div上时候，其内部图片会变大 */
div:hover img {
	transform: scaleX(1.5);
}
/* 回忆3d旋转图时候用到的,如果hover放在img上,img一直在变,会有bug */
```

下面两行代码容易混淆

```css
/*表示选择article所有后代的第一个子元素*/
/* 这里选中的是1，1.1，2.1，3.1 */
article :first-child {}
/* 等同于 */
article *:first-child {}

/*表示选择article为第一个子代的时候的article*/
article:first-child {}
```

```html
<article>
  <div>1
    <div>1.1</div>
    <div>1.2</div>
  </div>
  <div>2
    <div>2.1</div>
    <div>2.2</div>
  </div>
  <div>3
    <div>3.1</div>
    <div>3.2</div>
  </div>
</article>
```

##### 3.2 伪元素

伪元素选择的是特定情况下的内容（就好像给特定内容添加了一个`span`一样）。伪类只能以元素为整体选择，而伪元素可以选择元素指定内容，比如选择每段第一行，比伪类选择层次更深

```css
p::first-line {
	/* some code */
}
```

由于伪元素通常是更深层次的选择，所以和伪类一般这么组合

```css
p:first-child::first-line {
	/* some code */
}
```

伪元素还可用来插入一些特殊文本（屏幕阅读不识别的）

```css
p::after {
    content: " ➥"
}
```

伪元素还经常用来添加空字符串，这样可以设置任意样式。比如清除浮动啊，动画特效啊，阴影之类的很多功能都可以用这个实现

```css
div::before {
	content: "";
}
```

<br>

**Reference**：所有伪类和伪元素可查询[Selector](https://devdocs.io/css-selectors/)或者[MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements)

#### 4. 组合选择

|                   名称                   | 组合器  |                           选择                            |
| :--------------------------------------: | :-----: | :-------------------------------------------------------: |
|                 选择器组                 | `A, B`  |                         选中A和B                          |
|      后带选择器<br />（Descendant）      |  `A B`  | 选中B，满足条件：B是A的后代节点（所有的后代，包括孙子等） |
|        子代选择器<br />（Child）         | `A > B` | 选中B，满足条件：B是A的子节点（选中所有儿子。孙子等不选） |
| 相邻兄弟选择器<br />（Adjacent sibling） | `A + B` |      选中B，满足条件：B是A的兄弟节点，B要紧跟在A后面      |
| 通用兄弟选择器<br />（General sibling）  | `A ~ B` |          选中B，满足条件：B是A后面的任意兄弟节点          |

