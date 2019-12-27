#### 1. CSS是如何运行的

CSS运行流程如图所示：

1. 浏览器载入HTML文件
2. 将HTML文件转化为一个DOM（ Document Object Model ）
3. 浏览器加载HTML相关资源，如图片、视频和CSS样式等
4. 将获取的CSS解析，根据选择器对应的元素将样式应用在DOM节点中
5. 最后将样式后的HTML展示在屏幕上

  ![img](assets/rendering.svg ":size=600") 

#### 2. 关于DOM

一个DOM有一个树形结构，对应着HTML中每个节点。例

```html
<p>
  Let's use:
  <span>Cascading</span>
  <span>Style</span>
  <span>Sheets</span>
</p>
```

上述代码的DOM树如下

```
P
├─ "Let's use:"
├─ SPAN
|  └─ "Cascading"
├─ SPAN
|  └─ "Style"
└─ SPAN
   └─ "Sheets"
```