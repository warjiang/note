---
title: Animate.css使用 
tags: bower,animate.css,css动画
grammar_abbr: true
grammar_table: true
grammar_defList: true
grammar_emoji: true
grammar_footnote: true
grammar_ins: true
grammar_mark: true
grammar_sub: true
grammar_sup: true
grammar_checkbox: true
grammar_mathjax: true
grammar_flow: true
grammar_sequence: true
grammar_plot: true
grammar_code: true
grammar_highlight: true
grammar_html: true
grammar_linkify: true
grammar_typographer: true
grammar_video: true
grammar_audio: true
grammar_attachment: true
grammar_mermaid: true
grammar_classy: true
grammar_cjkEmphasis: true
grammar_cjkRuby: true
grammar_center: true
---
Animate.css相当于daneden给我们提供了一套CSS动画集,也有人称作为CSS框架
## 下载animate.css
你可以选择到animate.css的[github地址](https://github.com/daneden/animate.css)看下animate.css的介绍，并git clone下来
也可以使用bower来管理这些前端模块。这里我选择后者。
```markdown
bower install git://github.com/daneden/animate.css.git
```
## 开始使用
1. 在HTML的head标签中添加
```markdown
<head>
  <link rel="stylesheet" href="animate.min.css">
</head>
```

2. 在你想添加动画的元素上添加animated类。也可以增加```infinite ```的类来实现无线循环的动画。

3. 最后的话，你需要在下面类中添加一个类或多个类
    * bounce
    * flash
    * pulse
    * rubberBand
    * shake
    * swing
    * tada
    * wobble
    * bounceIn
    * bounceInDown
    * bounceInLeft
    * bounceInRight
    * bounceInUp
    * bounceOut
    * bounceOutDown
    * bounceOutLeft
    * bounceOutRight
    * bounceOutUp
    * fadeIn
    * fadeInDown
    * fadeInDownBig
    * fadeInLeft
    * fadeInLeftBig
    * fadeInRight
    * fadeInRightBig
    * fadeInUp
    * fadeInUpBig
    * fadeOut
    * fadeOutDown
    * fadeOutDownBig
    * fadeOutLeft
    * fadeOutLeftBig
    * fadeOutRight
    * fadeOutRightBig
    * fadeOutUp
    * fadeOutUpBig
    * flipInX
    * flipInY
    * flipOutX
    * flipOutY
    * lightSpeedIn
    * lightSpeedOut
    * rotateIn
    * rotateInDownLeft
    * rotateInDownRight
    * rotateInUpLeft
    * rotateInUpRight
    * rotateOut
    * rotateOutDownLeft
    * rotateOutDownRight
    * rotateOutUpLeft
    * rotateOutUpRight
    * hinge
    * rollIn
    * rollOut
    * zoomIn
    * zoomInDown
    * zoomInLeft 
    * zoomInRight
    * zoomInUp
    * zoomOut
    * zoomOutDown
    * zoomOutLeft
    * zoomOutRight
    * zoomOutUp
    * slideInDown
    * slideInLeft
    * slideInRight   
    * slideInUp
    * slideOutDown
    * slideOutLeft
    * slideOutRight
    * slideOutUp
