[babel](https://babeljs.io)

#### 1.方法1：引入JS文件

在线编译，但会降低代码性能。（就是把babel代码下载下来，在线编译），有很多缺陷，比如低版本浏览器就不识别`browser.min.js`

```html
<script src="browser.min.js" charset="utf-8"></script>
<script type="text/babel">
    let a = 12;
    console.log(a);
</script>
```

#### 2.方法2：编译JS文件

用的比较多的是这种方法

1. 安装Node.js、初始化项目（在目标文件夹下创建`package.json`）

   > npm init -y
   
2. 安装babel-cli
   > npm i @babel/core @babel/cli @babel/preset-env -D
   > npm i @babel/polyfill -S
   
3. 添加执行脚本（在`package.json`中添加）

   ```js
   "scripts": {
   	"build": "babel src -d dest"
   }
   // -d 就是destination 输出到指定文件夹
   // i 就是install缩写
   ```

4. 添加`.babelrc`配置文件

   ```js
   {
       "presets": ["@babel/preset-env"]
   }
   ```

5. 执行编译（就是执行`package.json`中的`build`）

   > npm run build