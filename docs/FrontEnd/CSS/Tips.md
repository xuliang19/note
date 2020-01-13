#### 1. 置换元素

一个内容不受CSS视觉格式模型控制，CSS渲染模型不考虑此内容的渲染，且元素本身一般拥有固定尺寸（宽度，高度，宽高比）

HTML中`img`、`input`、`textarea`、`select`、`object`等都是替换元素，这些元素往往没有实际的内容，是一个空元素

##### 1.1 object-fit

让图片宽度适应盒子宽度，除了用`width: 100%`，还可以使用`object-fit`。[MDN示例](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)

```css
object-fit: cover;/* 保持长宽比，让图片覆盖盒子 */
object-fit: contain;/* 保持长宽比，让图片在盒子内 */
object-fit: fill;/* 不会保持图片长宽比 */
```

#### 2. CSS 调试

如何调试CSS[参考MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Debugging_CSS)

#### 3. CSS一些注意

CSS语法规范，使用注意等[参考MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Organizing)

#### 4. display

隐藏元素可以用`display: none`或`visibility: hidden`，这两种方法的区别在于：前者不会占用空间；后者虽然隐藏元素，但是仍占据空间（也就是影响着布局）[示例](https://www.runoob.com/css/css-display-visibility.html)

#### 5. resize

CSS3中可用`resize`属性指定一个框，让用户去调整大小

  ```css
  div {
      resize:both;/* 还可取值horizontal vertical */
      overflow:auto;
  }
  ```

#### 6. outline

`outline`是不占用大小的（框在`boder`外层），用`outline-offset`来指定`outline`距离`border`的距离

#### 7. 命名

DIV+CSS命名，`container`，`eye-wrap`

#### 8. 经验

- CSS伪类`:target`用法，还可以`#news1:target`单独设置他的样式

```html
<!DOCTYPE html>
<html>
<head>
<style>
:target
{
border: 2px solid #D4D4D4;
background-color: #e5eecc;
}
</style>
</head>
<body>
<h1>这是标题</h1>
<p><a href="#news1">跳转至内容 1</a></p>
<p><a href="#news2">跳转至内容 2</a></p>
<p>请点击上面的链接，:target 选择器会突出显示当前活动的 HTML 锚。</p>
<p id="news1"><b>内容 1...</b></p>
<p id="news2"><b>内容 2...</b></p>
<p><b>注释：</b> Internet Explorer 8 以及更早的版本不支持 :target 选择器。</p>
</body>
</html>
```

- 实现微信小图标分享[w3school](https://www.w3schools.com/css/css_icons.asp)

