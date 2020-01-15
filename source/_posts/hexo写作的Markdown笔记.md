---
title: blog写作的一些笔记
tags: 
- blog
- note
abbrlink: 27208
date: 2019-06-22 18:14:37
categories:
---

# 写作介绍
## 字体介绍
*这是斜体* 或 _这也是斜体_ 
**这是粗体**
***这是加粗斜体***
~~这是删除线~~

## 超链接
写法：

行内形式：[我的博客](https://xfbxfbxfb.github.io/)
参考形式：[我的博客][1]，有一个很好的平台-[简书][2]
[1]:https://xfbxfbxfb.github.io/
[2]:http://www.jianshu.com/
自动链接：我的博客地址<https://xfbxfbxfb.github.io/>

<!--more-->
## 4,列表
无序列表：
写法：

* 无序列表项1
+ 无序列表项2
- 无序列表项3

有序列表：
写法：
1.有序列表项1
2.有序列表项2
3.有序列表项3

## 插入图片
在 Hexo 中插入图片，首先需要将图片放在 source/img/ 文件夹下，然后如下方式进行插入：
中括号中的内容是鼠标移到图片上时显示的描述。

![Markdown 创始人 John Gruber](/img/avatar.jpeg)

![](/img/avatar.jpeg)

> `<img src="..." width = "300" height"200" alt="图片标题">`

<img src="/img/2019-09-27_车窗上的雨滴,仿佛在水底.jpg" width = "300" height = "200" alt="车窗上的雨滴,仿佛在水底"/>

## 表格
| 表头1|表头2|表头3|表头4
|-| :- | :-: | -: |
|默认左对齐|左对齐|居中对其|右对齐|
|默认左对齐|左对齐|居中对其|右对齐|
|默认左对齐|左对齐|居中对其|右对齐|

##  Cite
> like this

## 代码标记
### 行内代码标记
just like `hello world!` and it's finished.

### 代码块标记
将需要标记的代码块选中，然后按下键盘上的 TAB 键。

```cpp
var executeSync = function(){
  var args = Array.prototype.slice.call(arguments);
  if (typeof args[0] === 'function'){
    args[0].apply(null, args.splice(1));
  }
};
```

## Link
[这是一个链接][1]
[1]: https://11.tt
[回到Cite](#7-Cite)

