#### 1. 背景样式

##### 1.1 background-color

定义背景颜色，还可以使用rgba值，设置半透明状态

##### 1.2 background-image

图片将在`background-color`上层显示，使用方式如下

```css
background-image: url('images/1.png')
```

可以使用多个背景图像，用逗号分隔。其他的`background-*`属性也用逗号分隔开，一一对应

```css
background-image: url(image1.png), url(image2.png), url(image3.png), url(image1.png);
background-repeat: no-repeat, repeat-x, repeat;
background-position: 10px 20px,  top right;
```

若其他属性匹配数量不够，那么就会循环匹配。如上，`imge3.png`的`repeat`应为`no-repeat`

##### 1.3 background-repeat

用于控制背景重复

- `no-repeat`：不重复
- `repeat-x`：水平重复
- `repeat-y`：垂直重复
- `repeat`：两个方向重复

##### 1.4 background-size

用于改变图像的大小（分别对应着图像的`width`和`height`）。值可以是数值、百分比或关键字。关键字`cover`表示保持图像比例，扩大直到覆盖盒子；`contain`表示保持图像比例，让图像大小适合盒子

```css
background-size: 10px 20%;
/* 图像width为10px,height为20% */
```

[MDN示例](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)

##### 1.5 background-position

用于改变背景图像位置，盒子左上角为坐标原点。两个值分别对应着X和Y轴坐标

值可以是数值，百分比或关键字（`top`,`center`,`right`等等）

```css
.box { 
  background-image: url('star.png'); 
  background-repeat: no-repeat; 
  background-position: top center; 
} 
```

[MDN示例](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)

还可以使用四个值，来表示和边缘的偏移量

```css
background-position: bottom 10px left 20px;
```

##### 1.6 background-attachment

控制当内容滚动时候，背景如何滚动

- `scroll`：默认值，盒子滚动背景不动，页面滚动背景动（相当于背景国定在页面上）
- `fixed`：盒子滚动背景不动，页面滚动背景不动
- `local`：盒子滚动背景滚动，页面滚动背景滚动

[MDN示例](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment)

##### 1.7 颜色渐变

用`background-image`设置，通过使用渐变函数

- 线性渐变：`linear-gradient()`
- 径向渐变：`radial-gradient()`
- 重复渐变：`repeating-linear-gradient()`,`repeating-radial-gradient()`

```css
/* 语法 */
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
/* 语法 */
background-image: linear-gradient(angle, color-stop1, color-stop2);
/* 语法 */
background-image: radial-gradient(shape size at position, start-color, ..., last-color);
/* 语法 */
```

[渐变在线生成](https://cssgradient.io/)

还可以通过设置百分比，来控制渐变的位置

```css
background: rgb(217,66,66);
background: linear-gradient(90deg, rgba(217,66,66,1) 0%, rgba(254,255,0,1) 37%, rgba(51,228,41,1) 47%, rgba(19,245,219,1) 54%, rgba(206,30,228,1) 100%); 
/* 后面百分比表示颜色终止位置 */
```

详细可参考[菜鸟教程](https://www.runoob.com/css3/css3-gradients.html)

##### 1.8 简写

基本语法

```css
background: [color] [image] [repeat] [attachment] [position] / [ size];
```

只需注意几点：`size`只能在`position`后面，并且用`/`分隔开；可以任意值里面的一个或多个值

`background`的覆盖范围是从`content`到`border`底下

#### 2. 边框

前面记录了，这里补充一`border-radius`

```css
div	{
    border-radius: 5px;
    /* 四个角都是半径为5px的圆角 */    
    border-top-right-radius: 1em 10%;
    /* 右上角,圆角半径水平值1em,垂直值10% */
}
```

