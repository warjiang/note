---
title: sass(scss)入门
tags: sass,scss
---
## sass分类
* .sass文件(不使用大括号和分号)
* .scss文件(使用大括号和分号)
推荐使用scss文件

## sass编译环境安装
1. 安装ruby
2. gem install sass
```markdown
PS.由于ruby的源放在AWS S3上,国内用户可以使用淘宝的ruby源
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://ruby.taobao.org
# 请确保只有 ruby.taobao.org
$ gem install sass
```

## 起步
1. sass使用(不使用大括号和分号)
    * test.sass
    ```markdown
    body
      background: #eee
      font-size: 12px
    p
      background: #0982c1
    ```
    * 编辑为CSS文件
    ```markdown
    sass test.sass test.css
    ```
2. scss使用(使用大括号和分号)
    * test.scss
    ```markdown
    //文件后缀名为scss的语法
    body {
      background: #eee;
      font-size:12px;
    }
    p{
      background: #0982c1;
    }
    ```
    * 编辑为CSS文件
    ```markdown
    sass test.scss test.css
    ```
## sass语法
* 导入
sass的导入(@import)规则和CSS的有所不同，编译时会将@import的scss文件合并进来只生成一个CSS文件。但是如果你在sass文件中导入css文件如@import 'reset.css'，那效果跟普通CSS导入样式文件一样，导入的css文件不会合并到编译后的文件中，而是以@import方式存在。
所有的sass导入文件都可以忽略后缀名.scss。一般来说基础的文件命名方法以_开头，如_mixin.scss。这种文件在导入的时候可以不写下划线，可写成@import "mixin"。
```scss
//1.css
#test{
    font-size: 12px;
}
//2.scss
body {
  background: #eee;
}
//import.scss
@import "1.css";
@import "2";
p{
  background: #0982c1;
}
//执行sass import.scss import.css
//生成import.css
@import url(1.css);
body {
  background: #eee; }

p {
  background: #0982c1; }

/*# sourceMappingURL=import.css.map */

//根据上面的代码可以看出，import.scss编译后，1.css继续保持import的方式，而2.scss则被整合进来了。
```
* 注释
sass有两种注释方式，
    * 一种是标准的css注释方式`/* */`.编译为CSS后，原样呈现
    ```css
    //mark1.scss
    /*
    *我是css的标准注释
    *设置body内距
    */
    body{
      padding:5px;
    }
    
    //编译后生成
    @charset "UTF-8";
    /*
    *我是css的标准注释
    *设置body内距
    */
    body {
      padding: 5px; }
    
    /*# sourceMappingURL=mark1.css.map */

    ```
    * 另一种则是`//`双斜杆形式的单行注释， 编译为CSS后消失。
    ```css
    //mark2.scss
    //我是双斜杠表示的单行注释
    //设置body内距
    body{
      padding:5px; //5px
    }
    //编译后生成
    body {
      padding: 5px; }
    /*# sourceMappingURL=mark2.css.map */
    ```
* 变量
    sass的变量必须是`$`开头，后面紧跟变量名，而变量值和变量名之间就需要使用冒号`:`分隔开（就像CSS属性设置一样），如果值后面加上`!default`则表示默认值。
1. 普通变量
    ```css
    //variable.scss
    $fontSize: 12px;
    body{
      font-size:$fontSize;
    }
    
    编译生成
    body {
      font-size: 12px; }
    
    /*# sourceMappingURL=variable.css.map */
    ```
2. 默认变量
    ```css
    //variabledefault.scss
    $baseLineHeight:        1.5 !default;
    body{
      line-height: $baseLineHeight;
    }
    //编译生成
    body {
      line-height: 1.5; }
    
    /*# sourceMappingURL=variabledefault.css.map */

    ```
3. sass默认变量的覆盖(在默认变量之前重新申明变量)sass的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可
    ```css
    //sass style
    //-------------------------------
    $baseLineHeight:        2;
    $baseLineHeight:        1.5 !default;
    body{
        line-height: $baseLineHeight; 
    }
    //编译生成
    body {
      line-height: 2; }
    //可以看出现在编译后的line-height为2，而不是我们默认的1.5。默认变量的价值在进行组件化开发的时候会非常有用。
    ```
4. 特殊变量
一般我们定义的变量都为属性值，可直接使用，但是如果变量在某些特殊情况下等则必须要以#{$variables}形式使用。
    ```css
    //sass style
    //-------------------------------
    $borderDirection:       top !default;
    $baseFontSize:          12px !default;
    $baseLineHeight:        1.5 !default;
    
    //应用于class和属性
    .border-#{$borderDirection}{
      border-#{$borderDirection}:1px solid #ccc;
    }
    //应用于复杂的属性值
    body{
      font-size:$baseFontSize/$baseLineHeight;
    }
    //编译生成
    .border-top {
      border-top: 1px solid #ccc; }
    
    body {
      font-size: 8px; }
    
    /*# sourceMappingURL=specialVariable.css.map */

    ```
5. 多值变量
    多值变量分为
    * list类型
        list数据可通过空格，逗号或小括号分隔多个值.
        * 取值nth($var,$index)//下表基于1
        * 求长度length($list)
        * join($list1,$list2,[$separator])
        * append($list,$value,[$separator])
        ```
        //一维数据
        $px1: 5px 10px 20px 30px;
        nth($px1, 2);//5px
        //二维数据，相当于js中的二维数组
        $px: 5px 10px, 20px 30px;
        $px: (5px 10px) (20px 30px);
        逗号和括号等效
        nth($px, 2)//20px 30px
        ```
        ```
        $linkColor:         #08c #333 !default;//第一个值为默认值，第二个鼠标滑过值
        a{
          color:nth($linkColor,1);
        
          &:hover{
            color:nth($linkColor,2);
          }
        }
        //生成
        a {
          color: #08c; }
          a:hover {
            color: #333; }
        
        /*# sourceMappingURL=list.css.map */

        ```
    * map类型
    map数据以key和value成对出现，其中value又可以是list。格式为：$map: (key1: value1, key2: value2, key3: value3);。可通过map-get($map,$key)取值。
    关于map数据还有很多其他函数如map-merge($map1,$map2)，map-keys($map)，map-values($map)等
    ```css
    $headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
    @each $header, $size in $headings {
      #{$header} {
        font-size: $size;
      }
    }
    //编译生成
    h1 {
      font-size: 2em; }
    
    h2 {
      font-size: 1.5em; }
    
    h3 {
      font-size: 1.2em; }

    /*# sourceMappingURL=map.css.map */
    ```
6. 全局变量
在变量值后面加上!global即为全局变量。
    ```css
    $fontSize:      12px;
    $color:         #333;
    body{
      $fontSize: 14px;
      $color:   #fff !global;
      font-size:$fontSize;
      color:$color;
    }
    p{
      font-size:$fontSize;
      color:$color;
    }

    //编译生成
    body {
      font-size: 14px;
      color: #fff; }
    
    p {
      font-size: 12px;
      color: #333; }
    /*# sourceMappingURL=global.css.map */
    ```
* 嵌套(Nesting)
    sass的嵌套包括两种：
    * 一种是选择器的嵌套
        所谓选择器嵌套指的是在一个选择器中嵌套另一个选择器来实现继承，从而增强了sass文件的结构性和可读性。
        在选择器嵌套中，可以使用&表示父元素选择器
        ```css
        #top_nav{
          line-height: 40px;
          text-transform: capitalize;
          background-color:#333;
          li{
            float:left;
          }
          a{
            display: block;
            padding: 0 10px;
            color: #fff;
        
            &:hover{
              color:#ddd;
            }
          }
        }
        //编译生成
        #top_nav {
          line-height: 40px;
          text-transform: capitalize;
          background-color: #333; }
          #top_nav li {
            float: left; }
          #top_nav a {
            display: block;
            padding: 0 10px;
            color: #fff; }
            #top_nav a:hover {
              color: #ddd; }
        
        /*# sourceMappingURL=nested.css.map */

        ```
    * 另一种是属性的嵌套
        ```css
        .fakeshadow {
          border: {
            style: solid;
            left: {
              width: 4px;
              color: #888;
            }
            right: {
              width: 2px;
              color: #ccc;
            }
          }
        }
        //编译生成
        .fakeshadow {
          border-style: solid;
          border-left-width: 4px;
          border-left-color: #888;
          border-right-width: 2px;
          border-right-color: #ccc; }

        /*# sourceMappingURL=netsted1.css.map */

        ```
* @at-root
    用来跳出选择器嵌套的。默认所有的嵌套，继承所有上级选择器，但有了这个就可以跳出所有上级选择器。
    * 普通跳出嵌套@at-root等价于@at-root (without:rule)
    ```css
    //没有跳出
    .parent-1 {
      color:#f00;
      .child {
        width:100px;
      }
    }
    
    //单个选择器跳出
    .parent-2 {
      color:#f00;
      @at-root .child {
        width:200px;
      }
    }
    
    //多个选择器跳出
    .parent-3 {
      background:#f00;
      @at-root {
        .child1 {
          width:300px;
        }
        .child2 {
          width:400px;
        }
      }
    }
    //编译生成
    .parent-1 {
      color: #f00; }
      .parent-1 .child {
        width: 100px; }
    
    .parent-2 {
      color: #f00; }
      .child {
        width: 200px; }
    
    .parent-3 {
      background: #f00; }
      .child1 {
        width: 300px; }
    
      .child2 {
        width: 400px; }
    
    /*# sourceMappingURL=at-root.css.map */
    我们使用@at-root内定义的属性，可以跳出根选择器
    ```
    * 其他层级`@at-root (without/with: all/media/support/rule)`分别是四个级别的跳出。`at-root就是at-root(without:rule)`
    ```css
    //跳出父级元素嵌套
    @media print {
      .parent1 {
        color: #f00;
        @at-root(without: rule)  {
          .child1 {
            width: 200px;
          }
        }
      }
    }
    
    //跳出media嵌套，父级有效
    @media print {
      .parent2 {
        color: #f00;
    
        @at-root(without: media)  {
          .child2 {
            width: 200px;
          }
        }
      }
    }
    
    //跳出media和父级
    @media print {
      .parent3 {
        color: #f00;
    
        @at-root (without: all) {
          .child3 {
            width: 200px;
          }
        }
      }
    }
    //编译生成
    @media print {
        .parent1 {
            color: #f00;
        }
    
        .child1 {
            width: 200px;
        }
    }
    
    @media print {
        .parent2 {
            color: #f00;
        }
    }
    
    .parent2 .child2 {
        width: 200px;
    }
    
    @media print {
        .parent3 {
            color: #f00;
        }
    }
    
    .child3 {
        width: 200px;
    }
    
    /*# sourceMappingURL=at-root1.css.map */
    //@at-root(without:media)指定跳出@media,但不跳出父级
    //@at-root(without:all)指定跳出@media和父级,等价于@at-root(without:rule media)
    ```
    * @at-root与&配合使用
    
    ```css
    .child{
        @at-root .parent &{
            color:#f00;
        }
    }
    //编译生成
    .parent .child {
      color: #f00; }
    
    /*# sourceMappingURL=at-root2.css.map */

    ```
* 混合(mixin)
    使用`@mixin`声明混合，可以传递参数，参数名以`$`符号开始，多个参数以逗号分开，也可以给参数设置默认值。声明的`@mixin`通过`@include`来调用。
    * 无参数
    ```css
    @mixin center-block {
      margin-left: auto;
      margin-right: auto;
    }
    
    .demo {
      @include center-block;
    }
    //编译生成
    .demo {
        margin-left: auto;
        margin-right: auto;
    }
    
    /*# sourceMappingURL=mixin.css.map */

    ```
    * 单一参数
    ```css
    @mixin opacity($opacity: 50) {
      opacity: $opacity / 100;
      filter: alpha(opacity=$opacity);
    }
    
    //css style
    //-------------------------------
    .opacity {
      @include opacity; //参数使用默认值
    }
    
    .opacity-80 {
      @include opacity(80); //传递参数
    }
    //编译生成
    .opacity {
        opacity: 0.5;
        filter: alpha(opacity=50);
    }
    
    .opacity-80 {
        opacity: 0.8;
        filter: alpha(opacity=80);
    }
    
    /*# sourceMappingURL=mixin1.css.map */

    ```
    * 多参数
    ```css
    @mixin horizontal-line($border: 1px dashed #ccc, $padding: 10px) {
      border-bottom: $border;
      padding-top: $padding;
      padding-bottom: $padding;
    }
    
    .imgtext-h li {
      @include horizontal-line(1px solid #ccc);
    }
    
    .imgtext-h--product li {
      @include horizontal-line($padding: 15px);
    }
    //编译生成
    .imgtext-h li {
        border-bottom: 1px solid #ccc;
        padding-top: 10px;
        padding-bottom: 10px;
    }
    
    .imgtext-h--product li {
        border-bottom: 1px dashed #ccc;
        padding-top: 15px;
        padding-bottom: 15px;
    }
    
    /*# sourceMappingURL=mixin2.css.map */

    ```
    * 多组值参数mixin
    ```css
    //box-shadow可以有多组值，所以在变量参数后面添加...
    @mixin box-shadow($shadow...) {
      -webkit-box-shadow: $shadow;
      box-shadow: $shadow;
    }
    
    .box {
      border: 1px solid #ccc;
      @include box-shadow(0 2px 2px rgba(0, 0, 0, .3), 0 3px 3px rgba(0, 0, 0, .3), 0 4px 4px rgba(0, 0, 0, .3));
    }
    //编译生成
    .box {
        border: 1px solid #ccc;
        -webkit-box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3), 0 3px 3px rgba(0, 0, 0, 0.3), 0 4px 4px rgba(0, 0, 0, 0.3);
        box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3), 0 3px 3px rgba(0, 0, 0, 0.3), 0 4px 4px rgba(0, 0, 0, 0.3);
    }

    /*# sourceMappingURL=mixin3.css.map */
    ```
* @content
    用来解决css3的@media等带来的问题。它可以使@mixin接受一整块样式，接受的样式从@content开始。建议传递参数的用@mixin，而非传递参数类的使用下面的继承
    ```css
    @mixin max-screen($res) {
      @media only screen and (max-width: $res) {
        @content;
      }
    }
    
    @include max-screen(480px) {
      body {
        color: red
      }
    }
    //编译生成
    @media only screen and (max-width: 480px) {
        body {
            color: red;
        }
    }
    
    /*# sourceMappingURL=@content.css.map */
    可以理解为@include max-screen($res){@content;}
    上面的调用为:
    $res = 480px;
    @content = body {
        color: red;
    }
    然后对应位置填充就是生成的代码
    ```
* 继承
    ```css
    h1{
      border: 4px solid #ff9aa9;
    }
    .speaker{
      @extend h1;
      border-width: 2px;
    }
    //编译生成
    h1, .speaker {
        border: 4px solid #ff9aa9;
    }
    
    .speaker {
        border-width: 2px;
    }
    
    /*# sourceMappingURL=extend.css.map */
    集成的部分采用联合声明,自身私有部分单独声明。
    ```
* 占位选择器%
    %选择占位符定义，@extend调用，与@mixin定义,@include调用类似。
    这种选择器的**优势**在于：如果不调用则不会有任何多余的css文件，避免了以前在一些基础的文件中预定义了很多基础的样式，然后实际应用中不管是否使用了@extend去继承相应的样式，都会解析出来所有的样式。相对于上面直接`@extend h1`而言。
    ```
    %ir {
      color: transparent;
      text-shadow: none;
      background-color: transparent;
      border: 0;
    }
    
    %clearfix {
      &:before,
      &:after {
        content: "";
        display: table;
        font: 0/0 a;
      }
      &:after {
        clear: both;
      }
    }
    
    #header {
      h1 {
        @extend %ir;
        width: 300px;
      }
    }
    
    .ir {
      @extend %ir;
    }
    //编译生成
    #header h1, .ir {
        color: transparent;
        text-shadow: none;
        background-color: transparent;
        border: 0;
    }
    
    #header h1 {
        width: 300px;
    }
    
    /*# sourceMappingURL=%25.css.map */
    //这里,我们通过%占位符定义了%ir和%clearfix,但是由于%clearfix没有使用@extend调用,所以在最终生成的css文件中不会看到clearfix的身影。占位选择器的出现，使css文件更加简练可控，没有多余。所以可以用其定义一些基础的样式文件，然后根据需要调用产生相应的css。
    ```
* 函数
    sass定义了很多函数可供使用，当然你也可以自己定义函数，以@fuction开始。sass的官方函数链接为：sass fuction，实际项目中我们使用最多的应该是颜色函数，而颜色函数中又以lighten减淡和darken加深为最，其调用方法为lighten($color,$amount)和darken($color,$amount)，它们的第一个参数都是颜色值，第二个参数都是百分比。
    ```
    $baseFontSize:      10px !default;
    $gray:              #ccc !defualt;        
    
    // pixels to rems 
    @function pxToRem($px) {
      @return $px / $baseFontSize * 1rem;
    }
    
    body{
      font-size:$baseFontSize;
      color:lighten($gray,10%);
    }
    .test{
      font-size:pxToRem(16px);
      color:darken($gray,10%);
    }
    //编译生成
    body {
        font-size: 10px;
        color: #e6e6e6;
    }
    
    .test {
        font-size: 1.6rem;
        color: #b3b3b3;
    }
    
    /*# sourceMappingURL=function.css.map */

    ```
* 运算
    ```
    $baseFontSize:          14px !default;
    $baseLineHeight:        1.5 !default;
    $baseGap:               $baseFontSize * $baseLineHeight !default;
    $halfBaseGap:           $baseGap / 2  !default;
    $samllFontSize:         $baseFontSize - 2px  !default;
    
    //grid
    $_columns:                     12 !default;      // Total number of columns
    $_column-width:                60px !default;   // Width of a single column
    $_gutter:                      20px !default;     // Width of the gutter
    $_gridsystem-width:            $_columns * ($_column-width + $_gutter); //grid system width
    ```
* 条件判断及循环
    * @if判断
    ```
    $lte7: true;
    $type: monster;
    .ib {
      display: inline-block;
      @if $lte7 {
        *display: inline;
        *zoom: 1;
      }
    }
    
    p {
      @if $type == ocean {
        color: blue;
      } @else if $type == matador {
        color: red;
      } @else if $type == monster {
        color: green;
      } @else {
        color: black;
      }
    }
    //编译生成
    .ib {
        display: inline-block;
        *display: inline;
        *zoom: 1;
    }
    
    p {
        color: green;
    }
    
    /*# sourceMappingURL=if.css.map */

    ```
    * 三目判断
    ```
    语法为：if($condition, $if_true, $if_false) 。
    三个参数分别表示：条件，条件为真的值，条件为假的值。
    if(true, 1px, 2px) //结果为1px
    if(false, 1px, 2px)//结果为2px
    ```
    * for循环
        $var表示变量，start表示起始值，end表示结束值，这两个的区别是关键字through表示包括end这个数，而to则不包括end这个数。
        * `@for $var from <start> through <end>`
        ```
        @for $i from 1 through 3 {
          .item-#{$i} {
            width: 2em * $i;
          }
        }

        ```
        * `@for $var from <start> to <end>`
    * @each循环
        语法为：`@each $var in <list or map>`。其中`$var`表示变量，而`list`和`map`表示`list类型数据`和`map类型数据`。
        * 单个字段list数据循环
        ```
        $animal-list: puma, sea-slug, egret, salamander;
        @each $animal in $animal-list {
          .#{$animal}-icon {
            background-image: url('/images/#{$animal}.png');
          }
        }
        //生成
        .puma-icon {
          background-image: url("/images/puma.png"); }
        
        .sea-slug-icon {
          background-image: url("/images/sea-slug.png"); }
        
        .egret-icon {
          background-image: url("/images/egret.png"); }
        
        .salamander-icon {
          background-image: url("/images/salamander.png"); }
        
        /*# sourceMappingURL=each1.css.map */

        ```
        * 多个字段list数据循环
        ```
        $animal-data: (puma, black, default), (sea-slug, blue, pointer), (egret, white, move);
        @each $animal, $color, $cursor in $animal-data {
          .#{$animal}-icon {
            background-image: url('/images/#{$animal}.png');
            border: 2px solid $color;
            cursor: $cursor;
          }
        }
        //生成
        .puma-icon {
          background-image: url("/images/puma.png");
          border: 2px solid black;
          cursor: default; }
        
        .sea-slug-icon {
          background-image: url("/images/sea-slug.png");
          border: 2px solid blue;
          cursor: pointer; }
        
        .egret-icon {
          background-image: url("/images/egret.png");
          border: 2px solid white;
          cursor: move; }
        
        /*# sourceMappingURL=each2.css.map */

        ```
        * 多个字段map数据循环
        ```
        $headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
        @each $header, $size in $headings {
          #{$header} {
            font-size: $size;
          }
        }
        //生成
        h1 {
            font-size: 2em;
        }
        
        h2 {
            font-size: 1.5em;
        }
        
        h3 {
            font-size: 1.2em;
        }
        
        /*# sourceMappingURL=each3.css.map */

        ```
            