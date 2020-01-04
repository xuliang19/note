CSS伪类`:target`用法，还可以`#news1:target`单独设置他的样式

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

2. `background`有关，`background`的覆盖范围是从`content`到`border`底下
