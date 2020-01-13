核心代码如下，要点：

1. 使用`focus`样式化选中样式
2. HTML结构`form`背景色应该用再外层的`.container`设置，而不是`form`
3. 其余详见[w3school 样式化表单](https://www.w3schools.com/css/css_form.asp)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            box-sizing: border-box;
        }
        input, select {
            /* display: block; */
            margin: 8px 0;
            padding: 8px 8px;
            width: 100%;
            border: 2px solid #cccc;
            border-radius: 5px;
            box-shadow: inset 0 1px 3px #ddd;
        }
        input:focus, select:focus {
            box-shadow: 0 0 8px rgba(81, 156, 194, 0.6);
        }
        div {
            background: #f2f2f2;
            padding: 40px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <form action="">
            <label>First Name<input type="text" name="first-name" placeholder="Your name.."></label>
            <label>Last Name<input type="text" name="last-name" placeholder="Your last name.."></label>
            <label>Country
                <select>
                    <option value="China">China</option>
                    <option value="America">America</option>
                    <option value="Canada">Canada</option>
                </select>
            </label>
        </form>
    </div>
</body>
</html>
```
