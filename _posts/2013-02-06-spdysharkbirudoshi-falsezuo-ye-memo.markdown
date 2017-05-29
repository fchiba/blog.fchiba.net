---
layout: post
title: "spdysharkビルド時の作業メモ"
date: 2013-02-06 19:56
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172483.html
---

SPDYのパケットを覗きたくて、spdyshark(WiresharkのSPDY plugin)をビルドしたらハマったのでメモ。結果的にはできてない。<br>
<br>
配布元の手順はここ。<br>
http://code.google.com/p/spdyshark/wiki/BriefInstallInstructions<br>
<br>
$ ./autogen.sh<br>
エラーが起きるので AG_CONFIG_HEADER -> AC_CONFIG_HEADERS に変更する<br>
<br>
configureするとライブラリがいろいろ足りないので、brewで追加。<br>
<br>
$ brew install gtk<br>
$ brew link --force cairo<br>
$ brew install pixman<br>
$ brew link --force pixman<br>
$ brew install freetype<br>
$ brew link --force freetype<br>
$ brew install fontconfig<br>
$ brew link --force fontconfig<br>
$ brew install libpng<br>
$ brew link --force libpng<br>
<br>
抜けあるかも。keg-onlyをlinkしまくってるが副作用は大丈夫か？？？<br>
<br>
$ PKG_CONFIG_PATH=/usr/X11/lib/pkgconfig:$PKG_CONFIG_PATH ./configure --with-ssl<br>
<br>
$ make<br>
libdissectors_la-packet-rtmpt.o で固まる<br>
<br>
$ CFLAGS=-O0 PKG_CONFIG_PATH=/usr/X11/lib/pkgconfig:$PKG_CONFIG_PATH ./configure --with-ssl<br>
<br>
ビルドはできて起動はする。が、SSLのデコード自体ができない。Why?<br>
最新のビルド済み WiresharkだとOK。<br>
<br>
ここで一旦断念。<br>


