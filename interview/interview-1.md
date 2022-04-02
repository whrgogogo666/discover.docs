HTML

1. 标签
  ● 行内元素：a b span img input select strong
  ● 块级元素：div ul ol li dl dt dd h1 h2 h3 h4 h5 h6 p
  ● 区别
  ○ 块级元素独占一行，默认垂直向下排列，行内元素不独占一行，都会在一条水平线上排列
  ○ 块元素height/width/margin/padding是可控的，设置有效，有边距效果，宽度没有设置时默认100%，行内元素height/width不可控，设置无效由内容决定
  ○ 块级元素可以包含块级元素和行内元素，行内元素最好只包含行内元素
  CSS
  盒子模型
2. 是什么？
  当对一个文档进行布局（layout）的时候，浏览器的渲染引擎会根据标准之一的 CSS 基础框盒模型（CSS basic box model），将所有元素表示为一个个矩形的盒子（box）。
3. 标准盒模型 IE盒模型

盒模型都是由四个部分组成的，分别是margin、border、padding和content
标准盒模型和IE盒模型的区别在于设置width和height时，所对应的范围不同：
● 标准盒模型的width和height属性的范围只包含了content，
● IE盒模型的width和height属性的范围包含了border、padding和content
通过修改元素的box-sizing属性来改变元素的盒模型
● box-sizing:content-box 标准盒模型
● box-sizing:border-box IE盒模型（怪异盒模型）
浮动

1. 只要添加了浮动，这个元素就会脱离标准流，如果父级元素盒子没有设置高度就会造成父级盒子的高度塌陷，会影响下面盒子的正常显示
  
  <head>
  <title>浮动</title>
  <style>
   .father{
     background-color: aquamarine;
   }
   .son1{
     width: 100px;
     height: 100px;
     background-color: blueviolet;
   }
   .son2{
     width: 100px;
     height: 100px;
     background-color: red;
   }
  </style>
  </head>
  <body>
  <div class="father">
   <div class="son1"></div>
   <div class="son2"></div>
  </div>
  </body>
  此时的父元素是被子元素撑起来了 显示边框黑色背景蓝色的区域
  

如果一旦两个子元素都设置为浮动了，那么就会出现，父级元素成了一条线了

2. 清除浮动的方法
  a. 给父级元素添加固定高度 （简单粗暴不建议使用，son盒子依然是脱离标准流，并没回到father盒子中）
  b. 在浮动元素末尾添加一个空标签（必须为块级元素），设置clear:both，这个属性作用是不允许周围有浮动现象，所以可以达到清除浮动的效果，但这样对于复杂的页面就显得结构非常乱，也不推荐使用。
  
  <head>
   <title>浮动</title>
   <style>
       .father{
           border: 2px solid black;
           background-color: aquamarine;
       }
       .son1{
           width: 100px;
           height: 100px;
           background-color: blueviolet;
           float: left;
       }
       .son2{
           width: 100px;
           height: 100px;
           background-color: red;
           float: left;
       }
       .clear{
           clear: both;
       }
   </style>
  </head>
  <body>
   <div class="father">
       <div class="son1"></div>
       <div class="son2"></div>
       <div class="clear"></div>  <!--添加清除浮动标签-->
   </div>
  </body>
  </html>
  
  c. 给父级添加overflow属性。BFC全称是块级格式化上下文，用于对块级元素排版，默认情况下只有根元素（body）一个块级上下文，但是如果一个块级元素设置了float:left,overflow:hidden或position:absolute样式，就会为这个块级元素生产一个独立的块级上下文，使这个块级元素内部的排版完全独立。也就是说独立的块级上下文可以包裹浮动流，全部浮动子元素也不会引起容器高度塌陷，就是说包含块会把浮动元素的高度也计算在内，这样就达到了清除浮动的效果，但是overflow:hidden本身的意思是溢出的元素隐藏显示，所以说有一定的缺点，大家可以根据情况来使用它。
  d. 给父级元素添加after伪元素 https://blog.csdn.net/FL_csdn1/article/details/113496670 
  元素水平垂直居中
  
3. 背景
  让某个元素的内容在水平和垂直方向上都居中，内容不仅限于文字，可能是图片或其他元素；
  可能分为两大类：居中元素的宽高已知 / 居中元素宽高未知
  
4. 实现方式
  //父级设为相对定位 子级设为绝对定位，四个定位属性设为0，
  //如果此时子级没有宽高则会跟父级一样宽高，如果有宽高则按设置的来，再设一个margin:auto就可以
  
  <style>
   .father{
       background-color: aqua;
       width: 200px;
       height: 200px;
       position: relative;
   }
   .son{
       background-color:red;
       width: 50px;
       height: 50px;
       position: absolute;
       top:0;
       left: 0;
       right: 0;
       bottom: 0;
       margin: auto;
   }
  </style>
  
  <body>
   <div class="father">
       <div class="son"></div>
   </div>
  </body>
  //外部元素设置 position: relative; 对内部的元素设置 position: absolute;
  //设置距上和左的距离为 50%，再通过设置 transform 分别向X轴和Y轴的位移 -50% ，实现水平处置居中
  <style>
   .father{
       background-color: aqua;
       width: 200px;
       height: 200px;
       position: relative;
   }
   .son{
       background-color:red;
       width: 50px;
       height: 50px;
       position: absolute;
       //这句话就是把子元素的中心放到父元素的左上角，见下图
       transform: translate(-50%, -50%);  //%是以自身为单位 
       //下边两句就是把此时的子元素放到父元素的中心
       top: 50%;
       left: 50%;
   }
  </style>
  

// display: flex时，表示该容器内部的元素将按照flex进行布局
// align-items: center表示这些元素将相对于本容器水平居中
// justify-content: center也是同样的道理垂直居中

<style>
    .father{
        background-color: aqua;
        width: 200px;
        height: 200px;
        display: flex;   //display: grid;
        align-items: center;
        justify-content: center;
    }
    .son{
        background-color:red;
        width: 50px;
        height: 50px;
    }
</style>

//设置父元素为display:table-cell，子元素设置 display: inline-block
//利用vertical-align和text-align可以让所有的行内块级元素水平垂直居中

<style>
    .father{
        background-color: aqua;
        width: 200px;
        height: 200px;
        display: table-cell;
        text-align: center;
        vertical-align: middle;
    }
    .son{
        background-color:red;
        width: 50px;
        height: 50px;
        display: inline-table;
    }
</style>

两栏布局实现(左栏固定，右栏自适应)
元素脱离文档流之后，将不再在文档流中占据空间，而是处于浮动状态（漂浮在文档流的上方）
● position
 ○ static 默认 正常文档流
 ○ relative 相对定位 不脱离文档流 只改变自身位置
 ○ absolute 绝对定位 相对于最近的父元素，脱离文档流
 ○ fixed 固定定位 相对于浏览器窗口 脱离文档流
//使用float会使元素脱离文档流，这样会导致两个div发生重叠
//在.right中设置overflow: hidden是为了触发BFC，清除浮动（不会重叠）

<style>
  .outer {
      border: 1px solid black;
  }
  .left {
      width: 200px;
      height: 200px;
      background:pink;
      float: left;         // 使用float会使元素脱离文档流，这样会导致两个div发生重叠
  }
  .right {
      height: 210px;
      background: orange;
      overflow: hidden;    // 设置overflow: hidden,触发BFC，BFC的区域不会与浮动元素发生重叠
  }
</style>

<div class="outer">
    <aside class="left"></aside>
    <div class="right"></div>
</div>
<style>
  .outer {
      border: 1px solid black;
  }
  .left {
      width: 200px;
      height: 200px;
      background:red;
      float: left;
  }
  .right {
      height: 210px;
      background: orange;
      margin-left: 200px;     //这里是left的宽度
  }
</style>
//定位 
<style>
    .outer {
        position: relative;    //相对定位
    }
    .left {
        width: 200px;
        height: 200px;
        background:red;
        position: absolute;    //绝对定位
    }
    .right {
        height: 210px;
        background: orange;
        margin-left: 200px;
    }
</style>

<style>
  .outer {
      position: relative;
  }
  .left {
      width: 200px;
      height: 200px;
      background:red;
  }
  .right {
      height: 210px;
      background: orange;
      position: absolute;
      top: 0;
      bottom: 0;
      right: 0;
      left: 200px;
  }
</style>

/*
flex 
flex-grow(定义放大比例):默认0 存在剩余空间 也不放大 1表示等分剩余空间 n表示占据的空间（放大的比例）是flex-grow为1的n倍
 flex-shrink(定义缩小比例)：默认1 空间不足缩小比例相同 0 空间不足时不缩小 n 空间不足时缩小的比例是flex-shrink为1的n倍
 flex-basis(定义在分配多余空间之前，项目占据的主轴空间):默认auto 即项目本来的大小 取值设置元素的宽度(可以覆盖width) 
*/

<style>
    .outer{
        display: flex;
    }
    .left {
        width: 200px;
        height: 200px;
        background-color: orange;
    }
    .right {
        height: 210px;
        background-color: red;
        flex: 1;     //flex表示 1 1 0%   等分剩余空间  空间不足缩小比例相同
    }
</style>

三栏布局实现

<style>
    .outer {
        border: 1px solid black;
    }
    .left {
        width: 200px;
        height: 200px;
        background: coral;
        float: left;
    }
    .right {
        width: 200px;
        height: 200px;
        background: lightblue;
        float: right;
    }
    .middle {
        height: 210px;
        background: lightpink;
        overflow: hidden;      //用overflow 或者 margin都可以
        /* margin-left: 200px;
        margin-right: 200px; */
    }
</style>

<div class="outer">
    <div class="left"></div>
    <div class="right"></div>
    <div class="middle"></div>     //这里必须要放到最后
</div>
<style>
    .outer {
        position: relative;
    }
    .left {
        width: 200px;
        height: 200px;
        background: coral;
        position: absolute;
    }
    .right {
        width: 200px;
        height: 200px;
        background: lightblue;
        position: absolute;
        top: 0;
        right: 0;         //靠右
    }
    .middle {
        height: 210px;
        background: lightpink;
        margin-left: 200px;
        margin-right: 200px;
    }
</style>
<style>
    .outer {
        border: 1px solid black;
        display: flex;
    }
    .left {
        width: 200px;
        height: 200px;
        background: coral;
    }
    .right {
        width: 200px;
        height: 200px;
        background: lightblue;
    }
    .middle {
        height: 210px;
        background: lightpink;
        flex: 1;
    }
</style>

<div class="outer">
    <div class="left"></div>
    <div class="middle"></div>      //这里middle一定要在中间
    <div class="right"></div>
</div>