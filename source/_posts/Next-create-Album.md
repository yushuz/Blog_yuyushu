---
title: hexo+Next搭建相册
tags:
  - hexo
categories:
  - hexo相关
abbrlink: 46820
date: 2019-12-29 17:25:40
---

喜欢hexo的Next主题，但是它不像yilia有默认相册页面，而blog内置相册是我的刚需。查阅了很多博文，最后基于以下两个做了自己的相册的测试版：
- [LizhiMa: 搭建Hexo博客相册](https://malizhi.cn/HexoAlbum/)
- [班班碎碎念： Hexo NexT 博客增加瀑布流相册页面](https://blog.dlzhang.com/posts/3720dafc/)

分别对应我的gallet-test1和gallery-test2。

应用过程中因为版本问题以及自己想要的一些细节不同，一些步骤并不相同，在这里记录一下。

<!-- more -->
# 搭建环境
```js
- hexo: 3.9.0
- Next: 7.6.0
- 鱼鱼书： 完全不懂前端
```

# 方法一
## 资源图片存放在github上
- 在任意位置创建一个新文件夹，例如我创建的叫album，在这个文件夹内执行`git init`创建git仓库。在Github上新建一个repository，随意命名，例如blog_blbum，复制这个repository的地址`https://github.com/yourname/repositoryname.git'，然后在本地仓库文件夹执行`git remote add origin https://github.com/yourname/repositoryname.git`。
- 下载[tool.py](https://github.com/yushuz/blog_album/blob/master/tool.py)至此文件夹内。（原文件[tool.py](https://github.com/malizhigithub/HexoAlbumData/blob/master/tool.py)有一些问题，这个tool.py是我改动后的）
- 在此文件夹下创建photos/文件夹，将想要展示的图片放在里面。图片需要以 year-month-day_description.xxx格式来命名（如2017-02-02_discriptionofyourpic.jpg），图片格式支持常见格式。（但我的jpeg图片不行。）
- tool.py会生成data.json文件，存储了图片链接，名称和简介。按照原文，在theme/next/source/lib中新建一个album来存放相册数据文件。将tool.py中约212行修改为这个用来存放相册数据data.json的路径。

```javascript
- with open("/home/orangecat/yuyushu_blog/themes/next/source/lib/album/data.json","w") as fp:
+ with open("YourBlogPath/themes/next/source/lib/album/data.json","w") as fp:
```
- 执行`python3 tool.py`。如果报错提示没有PIL库，执行`pip3 install Pillow`安装即可。执行后你会发现图片已经剪裁、压缩好并上传至github了。
## 创建相册页面
- 在博客根目录下执行`hexo new page album`，并在theme/next/_config.yml中为menu添加album:
```js
menu:
  home: / || home
  about: /about/ || user
  categories: /categories/ || th-list
  tags: /tags/ || tags
  archives: /archives/ || archive
+ photos: /photos  || camera
```
- 在source/album/index.md中添加以下代码：
```javascript
---
title: 照片
date: 2018-09-02 21:00:00
type: photos
fancybox: false
comments: true
---
<link rel="stylesheet" href="../lib/album/ins.css">
<link rel="stylesheet" href="../lib/album/photoswipe.css"> 
<link rel="stylesheet" href="../lib/album/default-skin/default-skin.css"> 
<div class="photos-btn-wrap">
  <a class="photos-btn active" href="javascript:void(0)" target="_blank" rel="external">Photos</a>
</div>
<div class="instagram itemscope">
  <a href="http://yourbolg.com" target="_blank" class="open-ins">图片正在加载中…</a>
</div>
 
<script>
  (function() {
    var loadScript = function(path) {
      var $script = document.createElement('script')
      document.getElementsByTagName('body')[0].appendChild($script)
      $script.setAttribute('src', path)
    }
    setTimeout(function() {
        loadScript('../lib/album/ins.js')
    }, 0)
  })()
</script>
```
## 资源文件配置
- 将[JS&CSS文件夹](https://github.com/malizhigithub/HexoAlbumData/tree/master/JS%26CSS)中所有文件都拷贝到前面创建的theme/next/source/lib/album/中。
- 将[photoswipe-ui-default.min.js](https://github.com/malizhigithub/HexoAlbumData/blob/master/photoswipe-ui-default.min.js)，[photoswipe.min.js](https://github.com/malizhigithub/HexoAlbumData/blob/master/photoswipe.min.js)两个文件添加到theme/next/source/js/src/中。没有对应文件夹就自己建一个。
- 将album中ins.js文件的约121行和122行修改为github中图片地址：
```js
-    var minSrc = 'https://raw.githubusercontent.com/lizhi/Blog_Album/master/min_photos/' + data.link[i];
-    var src = 'https://raw.githubusercontent.com/lizhi/Blog_Album/master/photos/' + data.link[i];
+    var minSrc = 'https://raw.githubusercontent.com/yougithubname/pathtoyourphotos/min_photos/' + data.link[i];
+    var src = 'https://raw.githubusercontent.com/yougithubname/pathtoyourphotos/photos/' + data.link[i];
```
为测试图片地址正确，可以选择其中一个图片地址看能不能在浏览器中打开。
## 引用以上配置
- 在next/layout/_layout.swig的head前添加对js文件的引用：
```js
<script src="{{ url_for(theme.js) }}/src/photoswipe.min.js?v={{ theme.version }}"></script>
<script src="{{ url_for(theme.js) }}/src/photoswipe-ui-default.min.js?v={{ theme.version }}"></script>
```
- 还是next/layout/_layout.swig文件，在body中添加：
```js
{% if page.type === "photos" %}
  <!-- Root element of PhotoSwipe. Must have class pswp. -->
  <div class="pswp" tabindex="-1" role="dialog" aria-hidden="true">
    <div class="pswp__bg"></div>
    <div class="pswp__scroll-wrap">
        <div class="pswp__container">
            <div class="pswp__item"></div>
            <div class="pswp__item"></div>
            <div class="pswp__item"></div>
        </div>
        <div class="pswp__ui pswp__ui--hidden">
            <div class="pswp__top-bar">
                <div class="pswp__counter"></div>
                <button class="pswp__button pswp__button--close" title="Close (Esc)"></button>
                <button class="pswp__button pswp__button--share" title="Share"></button>
                <button class="pswp__button pswp__button--fs" title="Toggle fullscreen"></button>
                <button class="pswp__button pswp__button--zoom" title="Zoom in/out"></button>
                <!-- Preloader demo http://codepen.io/dimsemenov/pen/yyBWoR -->
                <!-- element will get class pswp__preloader--active when preloader is running -->
                <div class="pswp__preloader">
                    <div class="pswp__preloader__icn">
                      <div class="pswp__preloader__cut">
                        <div class="pswp__preloader__donut"></div>
                      </div>
                    </div>
                </div>
            </div>
            <div class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap">
                <div class="pswp__share-tooltip"></div> 
            </div>
            <button class="pswp__button pswp__button--arrow--left" title="Previous (arrow left)">
            </button>
            <button class="pswp__button pswp__button--arrow--right" title="Next (arrow right)">
            </button>
            <div class="pswp__caption">
                <div class="pswp__caption__center"></div>
            </div>
        </div>
    </div>
  </div>
{% endif %}
```

完成！
## 一些问题
- 升级后的next没有next/layout/_scripts/pages/post-details.swig文件，修改/themes/next/layout/_partials/head.swig文件也会引起错误。折腾了很久之后发现原文的剩余步骤不用进行相册也可以正常运行。其中原因没有搞清楚。
- 因为我不想要vlog分支页面，所以我在photos/index.md中删掉了
```js
<div class="photos-btn-wrap">
  <a class="photos-btn active" href="javascript:void(0)" target="_blank" rel="external">Photos</a>
</div>
```

# 方法二
基本按照原文操作的。自己改了一些路径和现实细节。

# 方法三
其实就是写了一个新文章，在ggalery-test3里。在index.md的Front-matter里加一行`layout: post`即可。

另外，发现了一个插件： hexo-tag-google-photos-album，可以直接加入google相册，效果如下：
> 发现它会干扰别的图片加载。。

