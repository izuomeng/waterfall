# waterfall
实现类似于谷歌和百度图片的瀑布流布局页面，主要分为纯css方法和js方法

## css方法：
#### 1.column-count
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
       .content{
            column-count: 6;
       }
       .item{
           break-inside: avoid;
           background-color: lightgreen;
           margin-bottom: 10px;
       }
    </style>
</head>
<body>
    <div class="content">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>

    <script>
        let a = document.getElementsByClassName('item')
        for(let i = 0; i < a.length; i++){
            a[i].style.height = (Math.random() * 150 + 100) + 'px'
            a[i].style.backgroundColor = '#' + Math.random().toString(16).slice(2, 8)
        }
    </script>
</body>
</html>
```
