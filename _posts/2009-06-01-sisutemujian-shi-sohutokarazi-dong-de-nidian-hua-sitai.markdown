---
layout: post
title: "システム監視ソフトから自動的に電話したい"
date: 2009-06-01 23:07
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172442.html
---

システム監視(nagiosなど)で異常検知した場合に、自動的に電話したい。<br>
<br>
日中は携帯メールでもいいんだけど、寝ている最中に確実にたたき起こすにはやっぱ電話でしょ。<br>
お金のあるところなら、24時間オペレーターが張り付いて電話するんだろうけど、うちには金が無い。<br>
<br>
さてどうしたものか？<br>
<br>
１．携帯メールで我慢する。その代わり着信音を長めに設定する。<br>
　一番簡単に実現できるが、普段の着信音まで長くなる。<br>
　いちいち、みんなに設定させるのも手間。<br>
<br>
２．専用のサービスを使う。<br>
　電話とコンピュータの組み合わせのソリューションはたくあんあり、CTIというらしい。<br>
　が、コールセンターなどが主で、こんな小規模な用途向けのサービスはなさそう。<br>
<br>
３．モデムをおいて発信。<br>
　データセンターに電話線引くのが面倒。お金かかる。<br>
<br>
４．Skype API on Linux で電話に発信(SkypeOut)<br>
  APIでどこまで操作できるか調査が必要。APIからも SkypeOut とかできるのか？？？<br>
<br>
とりあえず最初は１．で。<br>
その間に４．を調査してみよう。<br>
<br>
とりあえず↓を買う予定<br>
<table style="width:75%;border:0;" border="0"><tr><td style="border:none;" valign="top" align="center"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4903408019/fchiba-22/ref=nosim/" target="_blank"><img src="http://ecx.images-amazon.com/images/I/41k51booKnL._SL75_.jpg" alt="Skype API Book Vol.1" border="0"></a></td><td style="padding:0 0.4em;border:0;" valign="top"><a href="http://blog.fc2.com/goods/4903408019/fchiba-22" target="_blank">Skype API Book Vol.1</a><br />(2006/08/23)<br />岩田 真一/rゆ/xai/池嶋 俊/大谷 弘喜/須崎 雅道/寺田 亮/谷萩 毅之/山本 達也<br /><br /><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4903408019/fchiba-22/ref=nosim/" target="_blank">商品詳細を見る</a></td></tr></table><br>


