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

