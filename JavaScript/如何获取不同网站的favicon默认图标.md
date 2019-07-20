# [现在网|现在博客-分享、记录生活的点滴](https://nowtime.cc/)


> ### favicon 通常是在浏览器中，网页标题前面显示的小图标来代表该网站的图标标记(俗称地址栏图标)。在一些地方（RSS 抓取应用、网站导航）需要显示网站的 favicon 图标，可以使用 Google 的服务来获取[国内可用！！！]，获取方式如下：

## 1. 服务器在国外

### API地址：

> [https://www.google.com/s2/favicons?domain=域名](https://www.google.com/s2/favicons?domain=%E5%9F%9F%E5%90%8D)

### 实例：

> 如果你想获取 `nowtime.cc` 的 favicon.ico
> 那么则访问：<https://www.google.com/s2/favicons?domain=nowtime.cc>

------

## 2. 服务器在国内

### API地址：

> [https://www.google.cn/s2/favicons?domain=域名](https://www.google.cn/s2/favicons?domain=%E5%9F%9F%E5%90%8D)

### 实例：

> 如果你想获取 `nowtime.cc` 的 favicon.ico
> 那么则访问：<https://www.google.cn/s2/favicons?domain=nowtime.cc>



# [利用公共api提取任意网站favicon.ico图标](https://blog.csdn.net/swanabin/article/details/46660433)

## 1、利用公共api提取任意网站favicon.ico图标

如何读取favicon

根据设置favicon的方式，就有2种读取favicon的方法：

 

A、默认直接读取网站根目录的favicon.ico文件。

B、如果不存在根目录下的favicon.ico文件，就读取页面里favicon的声明。

相比之下，获取网站根目录下的favicon.ico文件是最简单快捷的，但如果网站根目录下没有该文件，就需要使用后台程序读取网页的源代码，非常的麻烦。

 

 

为了克服获取favicon的麻烦，应运而生了一些获取favicon的公共API，如：

 

dnspod：http://statics.dnspod.cn/proxy_favicon/_/favicon?domain=url网址【特别推荐】

google：http://www.google.com/s2/favicons?domain=域名

getFavicon：http://www.getfavicon.org/?url=url地址

g.etfv.co：http://g.etfv.co/url地址

对网络速度而言，dnspod是国内的，快速并且稳定。谷歌的稳定性值得信赖，但因为时常在墙外，而不得不放弃。第3个getFavicon是早期获取favicon的网站，目前已经game over了。第4个也是国外的，也是经常在某些地区无法正常访问。第1和第2个胜出一筹。

 

对于传递参数而言，dnspod和谷歌都是传参域名，第3和第4个是传参url。第3和第4个胜出一筹。

 

总体而言，4个都打成了平手。

 

如此境遇下，我开发了一款获取网站favicon的公共API，只需要在传入网址即可获取图片，目前服务器设置在香港，无国界的访问，可以获取任何网址的favicon。并且，图片具有缓存30天的期限，第2次获取同一个域名（含多级域名）的favicon会更加快速。使用方法也很简单，如：

 

获取百度的favicon

 

http://*/?url=http://www.baidu.com

 

获取谷歌的favicon

 

http://*/?url=http://www.google.com

 

获取facebook的favicon

 

http://*/?url=http://facebook.com

 

获取github的favicon

 

http://*/?url=http://github.com

因流量以及滥用关系，现已经跳转到dnspod服务器的favicon获取方式，请使用该API的朋友尽快迁徙，该API将于今年年底彻底关闭。请使用http://statics.dnspod.cn/proxy_favicon/_/favicon?domain=网站地址 来获取该网站的favicon。

 

## 2、使用PHP获取网站Favicon的方法

最近做一个Tab需要在网站名旁边显示网站的Favicon以提高显示效果，如图：

[![icetab](http://7xlvm2.com1.z0.glb.clouddn.com/wp-content/uploads/2014/01/icetab.jpg)](http://7xlvm2.com1.z0.glb.clouddn.com/wp-content/uploads/2014/01/icetab.jpg)开始做的时候想到的是利用Google的方式来获取，使用“http://www.google.com/s2/favicons?domain=网址”的方式可以直接获得网站的Favicon图标并以16*16大小图片的形式显示出来，这个方法简单方便，但在有些网络环境下却会出现图片无法显示的问题（需要FQ），为了解决这个BUG我决定重新写一个获取Favicon的函数，使用自己的服务器以避免FQ。

实际效果请参见示例：

<http://favicon.byi.pw/?url=blog.icewingcc.com>

如果不想自己写方法的话也可以使用我提供的接口，即“http://favicon.byi.pw/?url=网址”，网址可以带http://前缀。

代码（调用Google的方式，这种方式可以减少代码量，并且速度也比较快）：

```php

<?php
  if(isset($_GET['url'])){
    $icon = file_get_contents("http://www.google.com/s2/favicons?domain=" . $_GET['url']);
    if($icon){
    	header('Content-type:image/png');
    echo $icon;
    }
  }
```



没错，就这几行代码搞定一切 ^_^

这样只要我们使用的服务器能够访问Google就可以正常显示出Favicon，不再受网络环境的影响。

复杂些的方法就是自己写获取函数，这里我只提供思路，就不再写代码了，如果有需要代码可留言，定附上。

一般网站都会把自己的Favicon图标以“favicon.ico”命名并放在网站根目录下，如http://www.baidu.com/favicon.ico。所以可以直接使用PHP函数 file_get_contents()来获取图片内容，设置Header为PNG图片，显示出来即可。

如果根目录没有favicon.ico这个文件的话可以使用file_get_contents或CURL获取网页的内容，使用正则找到“ <link rel=”shortcut icon” href=”..” />”，href里面便是favicon的文件位置，直接获取它的内容即可。