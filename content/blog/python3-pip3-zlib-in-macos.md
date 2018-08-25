+++
author = "Gary Gong"
categories = ["python3"]
tags = ["python3", "pip3", "macos", "zlib", "", ""]
date = 2018-08-23T09:32:15+08:00
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Cannot Use Pip3 in MacOS (zlib Dependency Problem)"
type = "post"

+++

### 問題描述

在使用 `pip3` 時，會回應 `zlib`相關套件的錯誤訊息

<!-- more -->

### 簡單解法

開 terminal 打

`xcode-select --install`

之後再裝

`brew install python3`

如果先裝 `python3` 然後再裝 `xcode-select` 東西的話

`brew reinstall python3`

就重裝 `python3` 吧