---
layout: post
title: "Android 4.4 (kitkat)のWebViewが速くなったらしいのでベンチマークをとってみた"
date: 2013-11-07 16:38
comments: 
categories: 
author: fumiya_chiba
permalink: archives/857609.html
---

Android 4.4 (kitkat)のWebViewが速くなったらしいのでベンチマークとってみました。<br /><br />■計測方法<br /><div>比較するアプリは、Chromeと「Eclipseで作ったBlankActivityにWebViewだけを突っ込んだもの」で、<a  href="http://v8.googlecode.com/svn/data/benchmarks/v7/run.html" target="_blank">v8 benchmark</a>&nbsp;のスコアを計測しました。また、target API Levelを19より小さくすることで互換モードで動くのですが、その影響もみています。&nbsp;リファレンス用に、Android4.2との端末でも測りました。<br /><br />■結果<br />Xperia AX (Android 4.2)</div><div>Chrome 2320</div><div>WebView(targetAPILevel=17) 1266</div><div>WebView(targetAPILevel=19) 1240</div><br /><div>Nexus5&nbsp;(Android 4.4)</div><div>Chrome 4119</div><div>WebView(targetAPILevel=17) 4465</div><div>WebView(targetAPILevel=19) 4356</div><div><br />■考察<br />- 以前はChromeに比べて大きく見劣りしていたWebViewの性能が、4.4以降ではChrome並みの性能になりました。</div><div>-&nbsp;target API Levelを下げても性能は同じでした。IEのように複数のレンダリングエンジンを持つのではなく、一つのエンジンの動作を変えているようです。<br />-&nbsp;Chrome単体よりWebViewが速いのは謎ですが、誤差みたいなものでしょう。<br /><br /></div>

