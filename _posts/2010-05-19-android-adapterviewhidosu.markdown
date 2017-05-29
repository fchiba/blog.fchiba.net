---
layout: post
title: "[Android]AdapterViewひどす"
date: 2010-05-19 16:10
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172468.html
---

AdapterViewを継承して、Galleryのような独自Viewを作ろうとしているのだが、困った事態に遭遇。<br>
<br>
AdapterViewの中に、mFirstPositionという、最初の表示されているアイテムの位置を表す変数がある。<br>
アイテムがどのように表示されるかはAdapterViewでは分からないので、継承したクラスでこの変数を更新する必要があるが、getterはある(getFirstVisiblePosition)のにsetterが存在しない！<br>
<br>
変数のスコープはデフォルト(=同パッケージ内からのみ参照可能)なので、Galleryなど既存のViewは直接変数を書き換えている。うーん、お行儀が悪い…<br>


