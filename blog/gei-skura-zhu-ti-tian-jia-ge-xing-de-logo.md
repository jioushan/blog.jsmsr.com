---
description: logo，css、Mashiro、有伴真白、白猫
---

# 🧑💻 给Skura主题添加个性的LOGO

## 细心的小伙伴发现。我的PC端 logo 像白猫那样。这篇文章讲述的就是如何做到这个样子。&#x20;

原文来自 [雾时之森](https://m1314.cn/212.html)&#x20;

### 首先你需要字体导入才能。启用这种字体。之后就是动画而已。&#x20;

### 1.那么对于适合的字体。（比如中文字体。或者日文字体）我推荐使用[Google字体](https://fonts.google.com/)&#x20;

### 2.之后你需要这个软件，[获取下载](https://ecomfe.github.io/fontmin/#app) （如果连接挂了，请评论留言）&#x20;

![](https://photo.jioushan.xyz/images/2020/04/17/1a377647cd0f15a16.png)

### 3. 生成完成后会有个文件夹，将文件夹复制到 Sakura主题包的 \inc\fonts 目录，文件夹重命名为你自己想要的名字，这里我用的是LOGO&#x20;

![](https://photo.jioushan.xyz/images/2020/04/17/2db212187bc6ce9e1.png)

### 4.复制其名字 打开Sakura主题包中的头文件 `header.php` 引用上面文件里的 KosugiMaru-Regular.css 文件&#x20;

```
<link rel="stylesheet" type="text/css" href="/wp-content/themes/Sakura/inc/fonts/LOGO/KosugiMaru-Regular.css">
```

### &#x20;修改 Sakura主题包中的头文件 `header.php` **class=logolin** 这个样式&#x20;

![](https://photo.jioushan.xyz/images/2020/04/17/39bb8bfe36c0f26de.png)

其样式名字为所引用字体名字 （当这个地方的引用的字体为软件生成的字体）&#x20;

### 5. 使用下面的代码。请自行修改。&#x20;

###

```
<span class="logolink KosugiMaru-Regular"> <a href="<?php bloginfo('url');?>"> <ruby> <span class="sakuraso">有坂</span> <span class="no">の</span> <span class="shironeko">ましろ</span> <rp></rp> <rt class="chinese-font"><?php echo akina_option('site_name', ''); ?></rt> <rp></rp> </ruby> </a> </span>
```

### 6.在 后台主题设置-自定义CSS样式增加如下CSS&#x20;

.KosugiMaru-Regular { font-family: 'miao', 'Merriweather Sans', Helvetica, Tahoma, Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft Yahei', 'WenQuanYi Micro Hei', sans-serif;; }

.logolink .sakuraso { background-color: rgba(255, 255, 255, .5); border-radius: 5px; color: #464646; height: auto; line-height: 25px; margin-right: 0; padding-bottom: 0px; padding-top: 1px; text-size-adjust: 100%; width: auto }

.logolink a:hover .sakuraso { background-color: orange; color: #fff; }

.logolink a:hover .shironeko, .logolink a:hover rt { color: orange; }

.logolink.KosugiMaru-Regular a { color: #464646; float: left; font-size: 25px; font-weight: 800; height: 56px; line-height: 56px; padding-left: 6px; padding-right: 15px; padding-top: 11px; text-decoration-line: none; } .logolink.KosugiMaru-Regular .sakuraso,.logolink.KosugiMaru-Regular.no { font-size: 25px; border-radius: 9px; padding-bottom: 2px; padding-top: 5px; } .logolink.KosugiMaru-Regular .no { display: inline-block; margin-left: 5px; }

.logolink a:hover .no { -webkit-animation: spin 1.5s linear infinite; animation: spin 1.5s linear infinite; }

.logolink ruby { ruby-position: under; -webkit-ruby-position: after; }

.logolink ruby rt { font-size: 10px; transform: translateY(-15px); opacity: 0; transiton-property: opacity; transition-duration: 0.5s, 0.5s; }

.logolink a:hover ruby rt { opacity: 1 } \</code>\</pre>&#x20;

### 请自行替换其中的**KosugiMaru-Regular**为您前面header.php所使用的class 名称&#x20;

#### 可以自定义颜色。&#x20;

### $注意，请将外观-Skura主题下的Logo取消图片。使用默认文字模式。否则样式无法表现！&#x20;
