# waterfall
实现瀑布流布局页面，主要分为纯css方法和js方法

## css方法：
#### 1.column-count
```html
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
<body>
    <div class="content">
        <div class="item"></div>
        <div class="item"></div>
        <!-- ...more items... -->
    </div>

    <script>
        let a = document.getElementsByClassName('item')
        for(let i = 0; i < a.length; i++){
            a[i].style.height = parseInt((Math.random() * 150 + 100)) + 'px'
            a[i].style.backgroundColor = '#' + Math.random().toString(16).slice(2, 8)
        }
    </script>
</body>
```
这个应该是最容易的办法了，简单明了，就是直接用column这个分栏特性把子元素分成固定的栏，可以结合媒体查询来优化不同规格显示屏的分栏数量，子元素是按照从上到下的“S”型顺序排列的，break-inside属性是核心，avoid可以避免尾部的div被截取，因此可以保持图片原来的大小

#### 2.flex
```html
<style>
   .content{
        display: flex;
        justify-content: space-around;
   }
   .column{
       display: flex;
       flex-flow: column;
       width: 24%
   }
   .item{
       width: 100%;
       background-color: lightgreen;
       margin-bottom: 10px;
   }
</style>
<body>
    <div class="content">
        <div class="column">
            <div class="item"></div>
            <!-- ...more items... -->
        </div>
        <div class="column">
            <div class="item"></div>
            <!-- ...more items... -->
        </div>
        <div class="column">
            <div class="item"></div>
            <!-- ...more items... -->
        </div>
        <div class="column">
            <div class="item"></div>
            <!-- ...more items... -->
        </div>
    </div>

    <script>
        let a = document.getElementsByClassName('item')
        for(let i = 0; i < a.length; i++){
            a[i].style.height = parseInt((Math.random() * 150 + 100)) + 'px'
            a[i].style.backgroundColor = '#' + Math.random().toString(16).slice(2, 8)
        }
    </script>
</body>
```
利用flex布局也可以实现瀑布流，不过这个更类似于伪瀑布流，因为每一列的图片数量是事先决定好的，并且没法自动让图片整体保持平衡，对于动态加载的图片来说不太适合这种方法，并且页面结构也比上一种要复杂，但是如果是静态展示网页的话可以考虑这么做

## js方法
#### js代码
```javascript
var Waterfall = (function (){
   var $scroll,
       $resize
   var Waterfall = function (content, boxClass) {
       var contentWidth = content.offsetWidth,
           boxWidth = getWidth(boxClass)   //获取boxClass类的宽度
       this.max = parseInt(contentWidth/boxWidth)   //最大列数
       this.space = contentWidth % boxWidth    //两侧的空白区域
       this.content = content
       this.boxClass = boxClass
       this.boxWidth = boxWidth
       this.columnHeight = Array(this.max).fill(0)  //保存每一列的最小高度值
   }
   Waterfall.prototype = {
       constructor: Waterfall,
       //添加图片方法，参数是要添加的张数，没有默认20张
       addImg: function(number) {  
           number = number || 20
           var div,
               len = this.content.children.length
           for(var i = 0; i < number; i++){
               div = document.createElement('div')
               div.innerHTML = len + i
               div.className = this.boxClass
               div.style.backgroundColor = '#' + Math.random().toString(16).slice(2, 8)
               div.style.height = parseInt(Math.random() * 200 + 200) + 'px'
               this.content.appendChild(div)
               this.shakeOne(div)
           }
           return this
       },
       //设置添加单个图片的位置符合瀑布流规则
       shakeOne: function(element) {
           var min = findMin(this.columnHeight),    //找到所有列中高度最小的值和索引
               margin = this.space / (this.max + 1)
           element.style.top = min.value  + 'px'
           element.style.left = min.index * (this.boxWidth + margin) + margin + 'px'
           this.columnHeight[min.index] += (element.offsetHeight + margin)  //更新列高度数组
           this.content.style.height = findMax(this.columnHeight) + 'px'   //维护父容器的高度
       },
       //整个页面重新布局
       refresh: function() {
           var children = this.content.children
           for(var i = 0; i < children.length; i++){
               this.shakeOne(children[i])
           }
       },
       //初始化，绑定监听事件，滚动到底部加载更多图片，resize窗口时重新布局
       init: function() {
           var tid1,
               tid2,
               body = document.body,
               self = this
           self.addImg(30)
           $scroll = function() {
               //函数截流
               clearTimeout(tid1)
               tid2 = setTimeout(function(){
                   if(body.scrollTop + window.innerHeight >= body.offsetHeight){
                       self.addImg(20)
                   }
               }, 200)
           }
           $resize = function() {
               clearTimeout(tid2)
               tid2 = setTimeout(function(){
                   //调整窗口大小时相应的参数也要改变
                   var contentWidth = self.content.offsetWidth
                   self.max = parseInt(contentWidth/self.boxWidth)
                   self.columnHeight = Array(self.max).fill(0)
                   self.space = contentWidth % self.boxWidth
                   self.refresh()
               }, 200)
           }
           document.addEventListener('scroll', $scroll)
           window.addEventListener('resize', $resize)
       },
       //移除监听事件
       remove: function() {
           document.removeEventListener('scroll', $scroll)
           window.removeEventListener('resize', $resize)
       }
   }
   function getWidth(boxClass) {
       var div = document.createElement('div')
       div.className = boxClass
       div.style.display = 'none'
       document.body.appendChild(div)
       return parseInt(getComputedStyle(div).width)
   }
   //辅助函数，找到ary数组中的最小值和索引，返回值是个对象
   function findMin(ary) {
       var min = {value: ary[0], index: 0}
       for(var i = 1; i < ary.length; i++){
           if(min.value > ary[i]){
               min.value = ary[i]
               min.index = i
           }
       }
       return min
   }
   //辅助函数，返回数组最大值
   function findMax(ary) {
       var max = 0
       for(var i = 0; i < ary.length; i++){
           if(max < ary[i]){
               max = ary[i]
           }
       }
       return max
   }
   return Waterfall
})()

var waterfall = new Waterfall(document.getElementById('1'), 'item')
waterfall.init()
```

js方法可以说是最强大的实现方法了，可以实现动态加载图片，而布局仍然维持瀑布流，这是css很难做到的，整体思路就是首先根据容器宽度和图片盒子宽度来确定最多能放置多少张图片，然后开始往里一张张放图片，同时用一个数组来维护当前每一列的高度，下标对应列的索引，每次放置的时候都要找到高度值最小的那一列和对应的索引，这样可以确定图片的绝对位置，left就是索引乘以图片盒子宽度，top就是这个最小的高度值，添加之后更新数组，完整的页面请见附件
