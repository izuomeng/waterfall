# waterfall
实现瀑布流布局页面，主要分为纯css方法和js方法

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
这个应该是最容易的办法了，简单明了，就是直接用column这个分栏特性把子元素分成固定的栏，可以结合媒体查询来优化不同规格显示屏的分栏数量，子元素是按照从上到下的“S”型顺序排列的，break-inside属性是核心，avoid可以避免尾部的div被截取，因此可以保持图片原来的大小
