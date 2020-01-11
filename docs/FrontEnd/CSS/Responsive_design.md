 待补全...

以前网站手机和电脑访问的不是同一个网址，兼容各客户端就要考虑响应式设计了。响应式设计的方式：可以根据不同的屏幕状态来更换CSS；也可以直接使用响应式布局（`flex`,`grid`布局，单位也使用百分比或者`em`等）

#### 1. responsive design

图片尺寸设置为`max-width:100%`

#### 2. media quries

[媒体查询实例](https://www.runoob.com/css3/css3-mediaqueries-ex.html)

用JS或CSS获取设备当前状态，从而采取不同的样式。例如使用CSS的`@media`

```css
/* 只有当viewport至少为800px时,里面的CSS才会应用 */
@media screen and (min-width: 800px) {
    .container {
        margin: 1em 2em;
    }
}
```

#### 3. flexible grids

在过去通常使用百分比这种布局来实现响应式

```css
.col { 
	width: 6.25%; /* 60 / 960 = 0.0625 */ 
} 
```

在现代浏览器中，直接使用`grid`,`flex`,`multicolumn`等响应式布局方式

#### 4. Responsive typography

还有各种技术，`font-size`以`em`为单位

```css
html { 
      font-size: 1em; 
} 
h1 { 
      font-size: 2rem; 
} 
@media (min-width: 1200px) { 
  h1 {
        font-size: 4rem; 
  }
} 
```

以`vw`为单位，缺点是用户无法放大文本，因为它的大小总是小对于`viewport`

```css
h1 {
      font-size: 6vw;
}
```

还可以使用`calc()`函数

```css
h1 {
    font-size: calc(1.5rem + 3vw);
}
```

#### 5. The viewport meta tag

```html
<meta name="viewport" content="width=device-width,initial-scale=1">
```

这个作用就是告诉浏览器设置他们的`viewport`宽度为设备的实际宽度，因为浏览器经常伪造`viewport`。**最好在所有HTML代码中都加上此条代码**。还有其他值可以设置：

- `initial-scale`: Sets the initial zoom of the page, which we set to 1.
- `height`: Sets a specific height for the viewport.
- `minimum-scale`: Sets the minimum zoom level.
- `maximum-scale`: Sets the maximum zoom level.
- `user-scalable`: Prevents zooming if set to `no`.

尽量避免使用`minimum-scale`,`maximum-scale`，也不要将`user-scalable`设置为`no`，用户会无法缩放