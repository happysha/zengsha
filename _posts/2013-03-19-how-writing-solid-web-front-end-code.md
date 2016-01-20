---
author: Summer
comments: true
date: 2013-03-19 05:39:36+00:00
layout: post
slug: how-writing-solid-web-front-end-code
title: 如何编写高质量css
wordpress_id: 1183
categories:
- 前端/应用开发
tags:
- css
- html
---

虽然写过很多css代码，但每次动手写都让我痛苦不堪，如何组织好那一堆堆的代码，如何提高代码复用率，甚至如何命名类，这些都让我纠结。下面的浅显的谈一谈在看了《编写高质量代码》中的html和css部分以及搜索网络上一些相关资料对编写高质量的HTML和CSS的一些基本认识。


## HTML


在说css之前必须先说说html ，没有它css是浮云。就像是要建栋别墅，光有那些装饰品（css）没有房子基本的结构（html）是不可能造出真正好看又实用的别墅的。其实html结构并不是简单的table或一堆堆的div，真正的高质量的html是具有良好语义化结构的，html5的header、nav和footer等这些新增的标签也是为良好语义化结构而生的吧。这就像让人鼻子是鼻子，眼睛是眼睛，而不是一顿乱放。


### 标签语义化


拥有良好语义化结构的前提是使用正确的标签。每一个html标签都有它自己的意义，下面是html的一张**HTML标签语义对照表**：


> div——division 分隔
span——span 范围
ol——ordered list 有序列表
ul——unordered list 无序列表
li——list item 列表项目
dl——definition list 定义列表
dt——definition term 定义术语
dd——definition description 定义描述
a——achlor 锚
strong——strong 加重
em——emphasized 加重
b——bold 加粗
i——italic 斜体
big——big 变大
small——small 变小
p——paragraph 段落
h1-h6——header 标题1到标题6
hr——horizontal rule 水平尺
sup——superscripted 上标
sub——subscripted 下标
font——font 字体
del——delete 删除
ins——inserted 插入
u——underlined 下划线
s——Strikethrough 删除线
fieldset——fieldset 域集
legend——legend 图标
caption——caption标题
abbr——abbreviation 缩写词
address——address 地址
var——varaiable 变量
pre——preformatted 预定义格式
blockquote——block quotation 区块引用语


**HTML5新增标签**


> <article> 标记定义一篇文章
<aside> 标记定义页面内容部分的侧边栏
<audio> 标记定义音频内容
<canvas> 标记定义图片
<command> 标记定义一个命令按钮
<datalist> 标记定义一个下拉列表
<details> 标记定义一个元素的详细内容
<dialog> 标记定义一个对话框(会话框)
<embed> 标记定义外部的可交互的内容或插件
<figure> 标记定义一组媒体内容以及它们的标题<footer> 标记定义一个页面或一个区域的底部
<header> 标记定义一个页面或一个区域的头部
<hgroup> 标记定义文件中一个区块的相关信息
<keygen> 标记定义表单里一个生成的键值
<mark> 标记定义有标记的文本
<meter> 标记定义 measurement within a
predefined range
<nav> 标记定义导航链接
<output> 标记定义一些输出类型
<progress> 标记定义任务的过程
<rp> 标记是用在Ruby annotations 告诉那些不支持 Ruby元素的浏览器如何去显示
<rt> 标记定义对ruby
annotations的解释
<ruby> 标记定义 ruby annotations.
<section> 标记定义一个区域
<source> 标记定义媒体资源
<time> 标记定义一个日期/时间
<video> 标记定义一个视频


现在我知道了每个标签的含义，那我怎么知道我的html语义是否良好？最好的办法是去掉css，去掉样式，看结构是否良好。可以在给网页布局时先把html写好，再去写相应的样式。没有样式的网页应该也具有良好的可读性。


## CSS


最开始写css就是看见什么写什么，从来不会想它还需要好好组织，最后代码一堆一堆，自己看着就头晕，发现兼容问题也是这里补补那里补补，毫无章法可言。现在才发现，css也是一门大学问。


### css结构


如何组织好css让代码结构更清晰、复用率更高？这需要有良好的css结构。 比较常见组织css的方法：base.css+common.css+page.css base层位于三者的最底层，提供CSS Reset和粒度最小的通用类——原子类，这一层的css可以被任何网站应用。 common层定义组件类，像button、标题栏这样特定的样式，这些组件类可以供整个网站使用. page层是相对于网站某个页面特有的样式，像主页可能就跟其他页面的样式有很大不同，你可以定义一个专用于首页的css（如index.css）。 不过话说回来，如果网站规模不大，我们也就没有必要把这些css分成三个文件（这样会增加请求数），可以把所有css统一放在一个文件里甚至html头部。不过我们还是需要规规矩矩地把它按照base、common、page这样的层级排列，并写上相应注释。


### css模块化


css模块化的目的是把具有相同样式的css组织起来，提高css的重用率。划分模块应该保证类的单一职责。**模块应在保证数量尽可能少的原则下，做到尽可能的简单，以提高重用率。 ** 举个例子： 我们来看一下新浪博客首页的四个列表，我们可以把这四个列表划分成两个完全一样的两大块（方形和圈）；但是我们发现其实这两大块中间的列表部分的样式其实是一样的，所以可以做如下调整。

![1-1](/wp-content/uploads/2013/03/1-1.png)

看下图，我们按照圈、箭头和方形所标注的三块内容就划分了三个不同的模块，这样每个模块的样式几乎是一样的（当然除了每个列表的名称），把列表作为一个模块单独出来。 ![1-2](/wp-content/uploads/2013/03/1-2.png)


### css命名


css命名也是一门学问。骆驼命名法和划线命名法的结合是不错的选择。骆驼命名法是从第二个单词开始首字母大写（如listItem、articleList）；划线命名法是每个单词用“_”或“-”分开（如list-item、article——list）。 我们可以用：**骆驼命名法用于区别不同单词，划线用于表明从属关系。 ** item属于list的子项目所以写成list_item的形式，articleList是指文章列表没有从属关系。最近看到一种BEM命名法，有兴趣的童鞋可以通过这篇文章了解一下《[BEM思想之彻底弄清BEM语法](http://www.w3cplus.com/css/mindbemding-getting-your-head-round-bem-syntax.html)》 当有很多相似但又不完全相同的模块时我们是给每一块加上不同的类然后使用css技巧把相同的部分提取出来（继承），还是让多个类组合成我们想要的效果呢？建议是多用组合，少用继承。


### css编码风格——多行式和一行式


多行式的编码风格可读性更强，但增加了css文件行数增大文件大小。一行式可读性稍差，但有效减少文件行数有利于提高开发速度。


### css兼容问题


我经常感觉被IE6“坑”，它会时不时给你些小惊喜。其实不止IE6，每个浏览器都有它特殊的一面。

补充阅读： 《[IE6下的几大灵异事件](http://www.oschina.net/question/179699_83795)》 《[最全的CSS浏览器兼容问题](http://68design.net/Web-Guide/HTMLCSS/37154-1.html)》 《[主流浏览器的Hack写法](http://www.w3cplus.com/css/browser-hacks.html)》


