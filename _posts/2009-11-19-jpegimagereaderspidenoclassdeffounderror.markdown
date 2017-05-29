---
layout: post
title: "JPEGImageReaderSpiでNoClassDefFoundError"
date: 2009-11-19 14:19
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172461.html
---

java.lang.NoClassDefFoundError<br>
at com.sun.imageio.plugins.jpeg.JPEGImageReaderSpi.createReaderInstance(JPEGImageReaderSpi.java:89)<br>
<br>
というエラーが出た場合の対処方法。<br>
<br>
http://forums.sun.com/thread.jspa?threadID=406987&messageID=2004167<br>
<br>
にある通り、画像関係のライブラリ(普通はXWindowと一緒に入る）がないため。<br>
<br>
yum install libXp<br>
yum install libXtst<br>
<br>
で動くようになった。<br>


