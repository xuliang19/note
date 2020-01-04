Media Query 是响应式布局的关键部分，它可以让你基于`viewport`创造不同的布局；还可以检测网站运行的环境（设备信息）

#### 1. Media Query基础

基本的语法如下：

```
@media media-type and (media-feature-rule) {
      /* CSS rules go here */
}
```

##### 1.1 Media types

- `all`
- `print`
- `screen`
- `speech`

例如，设置当打印时候，字体设为12pt

```css
@media print {
    body {
        font-size: 12pt;
    }
}
```

##### 1.2 Media feature rules

- Width and height

  ```css
  @media screen and (max-width: 400px) {
      body {
          color: blue;
      }
  }
  ```

- Orientation（方向）

  ```css
  @media (orientation: landscape) {
      body {
          color: rebeccapurple;
      }
  }
  /* 横向模式时候，颜色变为浅紫色 */
  ```

- Use of pointing devices

  由于触摸屏并没有`hover`，所以引用前最好检测一下

  ```css
  @media (hover: hover) {
      body {
          color: rebeccapurple;
      }
  }
  ```

#### 2. 复杂的Media Query

##### 2.1 逻辑“and”

`and`表示需满足所有条件才会应用样式

```css
@media screen and (min-width: 400px) and (orientation: landscape) {
    body {
        color: blue;
    }
}
```

##### 2.2 逻辑"or"

`,`表示满足其中一个条件即可

```css
@media screen and (min-width: 400px), screen and (orientation: landscape) {
    body {
        color: blue;
    }
}
```

##### 2.3 逻辑"not"

`not`表示反转选择范围

```css
@media not all and (orientation: landscape) {
    body {
        color: blue;
    }
}
```

#### 3. 选择breakpoint

在浏览器响应式布局里面有，可以选择设备，以确定什么时候该更换CSS样式