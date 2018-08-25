+++
author = "Gary Gong"
categories = ["廢文"]
tags = ["murmuring", "hexo", "mathjax", "katex"]
date = 2018-08-01T02:01:00+08:00
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "zzz我終於把 mathjax 弄進去了"
type = "post"

+++

我的老天，我花了一個多小時終於東湊西湊把 mathjax 塞進去了

搞到後來我都不知道我在幹嘛

結果 github 上面跟本機看得不一樣
<!-- more -->

這是 inline $ x^2+x-5=y $

這是隔行
$$
x^2+8
$$
累死我ㄌ

------

【2018/8/23 更】

我有試圖用過「hexo-renderer-markdown-it-plus」加「katex」，但是因為又喇了「hexo-math」套件，就形成 inline latex 給 hexo-math 渲染，隔行的 latex 給 katex 渲染。

但是 katex 在我試的時候 x 二次方的 2 跑版了，不得不放棄 katex；所以又換回「hexo-renderer-markdown-it」渲染，再搭配「hexo-math」。

然後在`_config.yml`安插一小段

```json
math:
   engine: 'mathjax'
   mathjax:
     src: "https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
     config:
         tex2jax:
             inlineMath: [ ['$','$'], ["\\(","\\)"] ]
             skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
             processEscapes: true
         TeX:
             equationNumbers:
				autoNumber: "AMS"
```

###### 無關緊要的後記

話說，我很想把 `*.yml`那個 yml 念成 やめろ，不覺得很像嗎？ya me ro