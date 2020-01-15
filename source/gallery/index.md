---
title: Gallery
date: 2017-12-29 22:32:22
type: gallery
---
<link rel="stylesheet" href="../lib/ins/ins.css">
<link rel="stylesheet" href="./instagram.min.css" />
<link rel="stylesheet" href="../lib/ins/photoswipe.css"> 
<link rel="stylesheet" href="../lib/ins/default-skin/default-skin.css"> 
<div class="instagram itemscope">
  <a href="http://yuyushu.online" target="_blank" class="open-ins">图片正在加载中…</a>
</div>

<script>
  (function() {
    var loadScript = function(path) {
      var $script = document.createElement('script')
      document.getElementsByTagName('body')[0].appendChild($script)
      $script.setAttribute('src', path)
    }
    setTimeout(function() {
        loadScript('../lib/ins/ins.js')
    }, 0)
  })()
</script>
