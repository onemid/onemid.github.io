+++
author = "Gary Gong"
categories = ["hexo"]

tags = ["hexo", "highlightjs", "prismjs", "tranquilpeak", "syntaxhighlight"]

date = "2018-08-23 17:24:27"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Hexo Syntax Highlighting (Tested under Tranquilpeak)"
type = "post"

+++

把這個主題弄 Syntax Highlighting 又是一番折騰，本來想說用`prism.js`做做，奈何就是在佈景上跟原始的佈景相衝，該高亮顯示的地方也沒有，索性走回頭路用`highlight.js`
<!-- more -->

基本上在 Hexo 佈景機制裡面加上一些 Code 就好，根據官網所述：

```html
<link rel="stylesheet" href="/path/to/styles/default.css">
<script src="/path/to/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

其中`/path/to/styles/default.css`基本上就是這個`highlight.js`的佈景即是， [https://highlightjs.org/static/demo/ ](https://highlightjs.org/static/demo/)這裡一票佈景，算是可以輕鬆找到要的。

如果說要用 CDN 的話，可以來這裡 [https://cdnjs.com/libraries/highlight.js/](https://cdnjs.com/libraries/highlight.js/) 翻翻，但是官網標配只含標準支援的程式碼 Highlighting 的功能，如果要額外的（比如說想用什麼 Julia 之類的），那得要自己去官網選配或是額外加 CDN，例如：

```html
...
<!-- 標準配備 -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/highlight.min.js"></script>
<!-- 選配 Julia -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/languages/julia.min.js"></script>
...
```

在 Hexo 佈景機制加上上述的東西，個人是放在 `themes/tranquilpeak/layout/_partial/footer.ejs`，可以動

比較重要的是，按理改完要在主題目錄下運行`npm run prod`，讓剛剛編輯的`footer.ejs`可以產出。

另外一個是，在 Hexo 根目錄下的`_config.yml`裡面：

```yaml
...
highlight:
	enable: false
...
```

記得要把 `enable` 改成 `false`，不然會打架。

改完之後，`hexo clean`和 `generate`以及`deploy`記得做ㄛ

#### 後記：使用深色佈景主題時，Tranquilpeak 會讓一些 Token 反白

我當時看到快傻眼了，怎麼那麼難搞阿Q

看了 Web Inspector 之後才發現 Tranquilpeak 有寫關於 Tag 的 CSS code，其中寫道：

```css
a.tag,
.tag {
  display: inline-block;
  background: #fff;
...
}
```

由於`highlight.js`把`<script>`、`<html>`等等這些含有「<」和「>」的東西統稱為`tag`們，在`class`理所當然理直氣壯的寫上去，然後就被 Tranquilpeak Theme 蓋了過去，所以難怪這些`tag`們的`background`都是白色的。

基本上，我的方案是這樣：

1. 先在主題根目錄把 `npm run start`開起來

2. 從主題根目錄往下找找到`source/_css/components/_tag.scss`

3. 把以下的程式碼打上去

   ```css
   pre .tag{
       background : none;
   }
   ```

4. 開心收工，記得`hexo clean`和 `generate`以及`deploy`

###### 無關緊要的後記

highlight.js 的官網滿滿的暗紅色看了眼睛好酸..