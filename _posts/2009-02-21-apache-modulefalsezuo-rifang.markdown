---
layout: post
title: "Apache Moduleの作り方"
date: 2009-02-21 01:51
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172436.html
---

・その１<br>
<a href="http://dsas.blog.klab.org/archives/50574774.html" target="_blank" title="apache module 開発事始め">apache module 開発事始め</a><br>
を読んで雰囲気をつかむ。<br>
<br>
・その２<br>
その間に<br>
<table style="width:75%;border:0;" border="0"><tr><td style="border:none;" valign="top" align="center"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/0132409674/fc2blog06-22/ref=nosim/" target="_blank"><img src="http://ecx.images-amazon.com/images/I/51sQph2MYyL._SL75_.jpg" alt="The Apache Modules Book: Application Development With Apache (Prentice Hall Open Source Software Development)" border="0"></a></td><td style="padding:0 0.4em;border:0;" valign="top"><a href="http://blog.fc2.com/goods/0132409674/fc2blog06-22" target="_blank">The Apache Modules Book: Application Development With Apache (Prentice Hall Open Source Software Development)</a><br />(2007/01/26)<br />Nick Kew<br /><br /><a href="http://www.amazon.co.jp/exec/obidos/ASIN/0132409674/fc2blog06-22/ref=nosim/" target="_blank">商品詳細を見る</a></td></tr></table><br>
を注文して読む。<br>
<br>
・その３<br>
APRに関しては、<br>
<a href="http://apr.apache.org/docs/apr/0.9/group__APR.html" target="_blank" title="リファレンス">リファレンス</a><br>
と<br>
<a href="http://dev.ariel-networks.com/apr/apr-tutorial/html/apr-tutorial.html" target="_blank" title="libapr(apache portable runtime) programming tutorial">libapr(apache portable runtime) programming tutorial</a><br>
を読めば、だいたいわかる。<br>
ディープなところは、<br>
https://www.codeblog.org/blog/inoue/<br>
を参考にした。<br>
<br>
・その４<br>
今回はC++のライブラリなどを使うので、<br>
http://d.hatena.ne.jp/dayflower/20080904/1220510124<br>
を参考に（というかほぼ全コピー）させてもらいMakefileを作った。<br>
<br>
・その５<br>
あとは、既存のモジュールを読む。XMLをごにょごにょするフィルタを作りたかったので<br>
modxslt<br>
mod_transform<br>
mod_ext_filters<br>
などを主に参考にした。<br>
<br>
（続く）<br>


