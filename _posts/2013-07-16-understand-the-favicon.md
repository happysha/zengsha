---
author: happysha
comments: true
date: 2013-07-16 05:38:00+00:00
layout: post
slug: understand-the-favicon
title: 【转】弄懂Favicon
wordpress_id: 2788
categories:
- 一周一博
- 前端/应用开发
- 设计
- 转载
tags:
- favicon
---

> favicon是favorite icon的缩写，通常会显示在浏览器收藏夹（即书签）中以及地址栏左侧。中文有译作网站头像或书签图标，此处译作网站图标。

——译者：[David](http://t.qq.com/lybluesky0110)


当[Alec Rust](https://twitter.com/AlecRust)问及[HTML5 Boilerplate](http://html5boilerplate.com/)项目是否可以提供[切换成高DPI的favicon](https://github.com/h5bp/html5-boilerplate/issues/1285)的时候，我才意识到我对网站图标（[favorite icon](http://www.w3.org/2005/10/howto-favicon)），触摸图标（[touch icons](http://developer.apple.com/library/ios/#documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)），和磁贴图标（[tile icons](http://blogs.msdn.com/b/ie/archive/2012/06/08/high-quality-visuals-for-pinned-sites-in-windows-8.aspx)）知之甚少。当我决定稍稍深究一下的时候却发现一些有趣的事情。

自从IE在1999年首次引入favicon以来，之后它就没再发生过什么变化。它们几乎总是[ICO文件](http://en.wikipedia.org/wiki/ICO_(file_format))，并且放在网站的根目录下，如：/favicon.ico，或者在CMS系统中作为某个主题的favicon放在图片目录下，就像下面这样：

    
    <link rel="shortcut icon" href="/path/to/favicon.ico">


![弄懂Favicon](http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/2013/favicon-1.jpg)

传统的favicon.ico是一个16×16像素的ICO文件，通常是16位色或带alpha透明度的24位色格式。当下也有32×32像素的favicon被使用，它们在主流浏览器中会被适当地缩小。在Metro版的IE 10中，地址栏上就是使用了32×32的图标

![弄懂Favicon](http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/2013/favicon-2.jpg)

rel属性是favicon演变中的一个产物。IE 5本打算用shortcut icon来表示页面和图标之间的关系，但是当这份[规范](http://tools.ietf.org/html/rfc5988)把rel属性值所表示的关系用空格分开后，这样理论上shortcut icon是创建了两种关系，即shortcut和icon。直到2010年HTML5规范把icon单独作为标准标识符。所以，在非IE浏览器中指定favicon时，可以不加shortcut属性。

    
    <!-- IE6-10 -->
    <link rel="shortcut icon" href="path/to/favicon.ico">
    
    <!-- Everybody else -->
    <link rel="icon" href="path/to/favicon.ico">


favicon的type属性和[<script>的type属性](http://www.w3.org/html/wg/drafts/html/master/scripting-1.html#attr-script-type)的作用几乎一样。截至2013年1月16日，据[维基百科](http://en.wikipedia.org/wiki/Favicon)显示，无论客户端是否为IE浏览器，[favicon的type属性](http://en.wikipedia.org/wiki/Favicon#How_to_use)都可能对其正常显示产生影响。事实上，IE只关心服务器MIME类型要为ICO文件，却会忽略type属性。因此，type属性可以是任意值，也可以为空。

    
    <!-- Still works in IE6+ -->
    <link rel="shortcut icon" href="path/to/favicon.ico" type="image/vnd.microsoft.icon">
    
    <!-- Still works in IE6+ -->
    <link rel="shortcut icon" href="path/to/favicon.ico" type="image/x-icon">
    
    <!-- Still works in IE6+ -->
    <link rel="shortcut icon" href="path/to/favicon.ico">


![弄懂Favicon](http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/2013/favicon-3.jpg)

令我沮丧的是，Chrome，Firefox，Opera 7+，和Safari 4+都支持PNG格式的favicon，但是Chrome和Safari在两种格式都提供的情况下却会使用ICO格式，而且完全无视favicon的声明顺序。另一方面，IE不支持PNG格式的favicon，但是它会忽略PNG格式的声明以及声明顺序而直接使用ICO格式的favicon。

    
    <!-- Chrome, Safari, IE -->
    <link rel="shortcut icon" href="path/to/favicon.ico">
    
    <!-- Firefox, Opera (Chrome and Safari say thanks but no thanks) -->
    <link rel="icon" href="path/to/favicon.png">


就像ICO格式一样，一个PNG格式的favicon也不适用于多种分辨率，所以我们可以写上多个含有sizes属性的声明来匹配每一种分辨率。

    
    <link rel="icon" href="favicon-16.png" sizes="16x16">
    <link rel="icon" href="favicon-32.png" sizes="32x32">
    <link rel="icon" href="favicon-48.png" sizes="48x48">
    <link rel="icon" href="favicon-64.png" sizes="64x64">
    <link rel="icon" href="favicon-128.png" sizes="128x128">


那些兼容PNG格式favicon的浏览器是如何确定使用哪个favicon的？Firefox和Safari会使用最后声明的那个。Chrome for Mac只会使用32×32大小的ICO格式。Chrome for Windows会首选16×16大小的ICO格式。如果在没有上述选项的情况下，两种平台的Chrome都会使用第一个声明的favicon，这点正好与Firefox和Safari相反。Chrome for Mac确实会忽略16×16的favicon而直接使用32×32的，如果在非视网膜屏的设备上就会把它缩小到16×16。Opera则完全是一种中立的态度，它会从所有声明的可用的favicon中完全随机地选择一个。我喜欢Opera这种做法。

以上说的仅仅只是开始，现在是时候让我们来了解一下IE中的注意事项。

IE8-10会在页面首次加载的时候就显示favicon。IE7则不会在页面第一次加载的时候就显示，而是在页面再次被访问的时候才显示favicon。更糟的是，IE6只会在网站被添加到收藏夹中并且在浏览器再次打开的时候才会显示favicon。只要浏览器清除缓存，favicon就会被删除。并且只有把网站再次添加的收藏夹中，favicon才会再次显示，或者是用某种手段再加载一次favicon文件。如果你不得不和IE6以及favicon同时打交道，那么你可以用下面这段JavaScript代码来强制重新加载favicon文件，最好把这段代码包裹在条件注释中。

    
    <!-- I "support" IE6 -->
    <!--[if IE 6]><script>(new Image).src="path/to/favicon.ico"</script><![endif]-->


让我们回到高DPI的问题上来。


> 你有没有问过自己这样一个问题——如果所有好浏览器都支持PNG格式的favicon，并且IE只需要提供ICO格式，但是ICO格式对Chrome和Safari却是个特例，那我们为什么不把ICO格式的favicon声明包裹在IE的条件注释中？


好想法总为解决大难题而诞生。PNG文件可以代替ICO文件为你提供更大的图标。我们可以在IE中使用一个经典的32×32的ICO格式的favicon，而在除IE以外的其他浏览器中使用一个超级圆滑的96×96的PNG格式的favicon。

    
    <!-- Just IE? -->
    <!--[if IE]><link rel="shortcut icon" href="path/to/favicon.ico"><![endif]-->
    
    <!-- Everybody else? -->
    <link rel="icon" href="path/to/favicon.png">


现在有个大问题是，[IE 10不再支持条件注释](http://www.sitepoint.com/microsoft-drop-ie10-conditional-comments)，而且也不支持PNG格式的favicon。上面的代码在之前的IE浏览器中确实会比最新的IE浏览器有更好的效果。


> 如果你说“嘿——要是我们把ICO格式的favicon放到网站的根目录下，而只使用<link rel="icon">来声明PNG格式的favicon会怎么样？”


恭喜你，你赢了。根据Chrome，Safari，和IE的局限性，这个办法确实可以让每个浏览器获得最好的favicon效果。IE会忽略<link rel="icon">这样的声明而自动获取网站根目录/favicon.ico的ICO格式的favicon。而其他所有浏览器则会使用如下声明的PNG格式的favicon：

    
    <link rel="icon" href="path/to/favicon.png">




> 你又会说“但是如果我想要多个favicon或者我的CMS系统不喜欢这么做，还有……其他的办法么？”


当然有，不过你可能不会喜欢

    
    <!-- I "support" IE -->
    <script>
    navigator.appName == "Microsoft Internet Explorer" && (function (i, d, s, l) {
       i.src = "favicon.ico";
       s = d.getElementsByTagName("script")[0];
       l = s.parentNode.insertBefore(d.createElement("link"), s);
       l.rel = "shortcut icon";
       l.href = i.src;
    })(new Image, document);
    </script>
    
    <!-- Everybody else -->
    <link rel="icon" href="path/to/favicon.png">




> 上面这段JS脚本的意思是当客户端是IE浏览器时，构造了一条ICO格式的favicon声明，即:

>     
>     <linkrel="shortcut icon"href=" favicon.ico">
> 
> 
并插入页面文档中。——译者：[David](http://t.qq.com/lybluesky0110)


对上面的解决方案都不满意？没关系，还有办法。[目前](http://windows.microsoft.com/en-US/internet-explorer/ie-10-release-preview)使用IE 10的大多是Windows 8的用户。Windows 8提出一种新的网站显示图标——磁贴图标。

![弄懂Favicon](http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/2013/favicon-4.jpg)

在Metro版的IE 10中，当网站的访问者把我们的网站附到（pin）开始屏幕上的时候，我们可以显示一个独一无二的磁贴图标。这些磁贴图标是144×144大小的PNG文件，为达到最佳效果它们使用透明背景。磁贴图标的背景色是可以用十六进制的RGB色（使用#RRGGBB这样的6位符号记法），或者CSS中的颜色名称，或者是CSS中的rgb()函数来指定。声明磁贴图标的标签非常简单。

    
    <meta name="msapplication-TileColor" content="#D83434">
    <meta name="msapplication-TileImage" content="path/to/tileicon.png">


![弄懂Favicon](http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/2013/favicon-5.jpg)

OK，现在让我们来把它们合在一起，既满足IE 10的局限性，又保证其他方面的健全。

    
    <link rel="apple-touch-icon" href="path/to/touchicon.png">
    <link rel="icon" href="path/to/favicon.png">
    <!--[if IE]><link rel="shortcut icon" href="path/to/favicon.ico"><![endif]-->
    <!-- or, set /favicon.ico for IE10 win -->
    <meta name="msapplication-TileColor" content="#D83434">
    <meta name="msapplication-TileImage" content="path/to/tileicon.png">


至少这是个不错的开始。

如果你对创建favicon想了解更多，我推荐你看看Jon Hick的《[图标指南](http://www.netmagazine.com/features/create-perfect-favicon)》（[The Icon Handbook](http://www.fivesimplesteps.com/products/the-icon-handbook)）中的创造完美的favicon，以及Jonathan Snook的制作[好的favicon](http://snook.ca/archives/design/making_a_good_favicon)。此外，我想感谢[@alrra](http://twitter.com/alrra)告诉我关于磁贴图标的内容。

**译者手语：**整个翻译依照原文线路进行，并在翻译过程略加了个人对技术的理解。如果翻译有不对之处，还烦请同行朋友指点。谢谢！


> 

> 
> #### 关于David
> 
> 
2009年开始接触前端开发，2011年组建创业团队——[[五维互动](http://www.fi5e-d.com/)]，2012年团队被“收编”并更名[创影互动]，遂只身来上海发展，现在就职于[FlipScript](http://www.flipscript.com.cn/)。欢迎交流共勉：[腾讯微博](http://t.qq.com/lybluesky0110)、[个人博客](http://www.codingserf.com/)。


如需转载烦请注明出处：

英文原文：[http://www.jonathantneal.com/blog/understand-the-favicon/](http://www.jonathantneal.com/blog/understand-the-favicon/)

中文译文：[http://www.w3cplus.com/css/understand-the-favicon.html](http://www.w3cplus.com/css/understand-the-favicon.html)
