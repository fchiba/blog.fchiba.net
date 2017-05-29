---
layout: post
title: "Baculaを試す"
date: 2009-03-23 15:56
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172440.html
---

以前、<br>
<a href="http://trombik.mine.nu/~cherry/w/index.php/2008/08/03/1333/use-bacula" target="_blank" title="バックアップならBaculaでしょ">バックアップならBaculaでしょ</a><br>
を読んで気になっていたBaculaを試してみた。<br>
<br>
おもに参考にしたのは、<br>
<table style="width:75%;border:0;" border="0"><tr><td style="border:none;" valign="top" align="center"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4774136352/fc2blog06-22/ref=nosim/" target="_blank"><img src="http://ecx.images-amazon.com/images/I/514%2B9TZfJAL._SL75_.jpg" alt="WEB+DB PRESS Vol.47" border="0"></a></td><td style="padding:0 0.4em;border:0;" valign="top"><a href="http://blog.fc2.com/goods/4774136352/fc2blog06-22" target="_blank">WEB+DB PRESS Vol.47</a><br />(2008/10/24)<br />WEB+DB PRESS編集部<br /><br /><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4774136352/fc2blog06-22/ref=nosim/" target="_blank">商品詳細を見る</a></td></tr></table><br>
と<br>
<a href="http://lunatear.net/archives/cat_14_bacula.html" target="_blank" title="LunaTear: Bacula Category">LunaTear: Bacula Category</a><br>
<br>
設定ファイルを書く手順としては、後者で勧められている通り、末端のfdやsdから書いていき、最後にdirectorを書くのが楽だと思う。<br>
<br>
ちなみに、インストールした先はCentOS5.2。<br>
パッケージを使いたかったが、epelにもあるのが古かったので、sourceforgeにあるrpms-contrib-fschwarzのel5を使用した。<br>
パッケージの種類はいろいろあるが、bacula-mtxとbacula-sqliteだけインストールしたら動いた。<br>
bacula-mysqlとかbacula-postgresqlとかはカタログの保存先をDBにするときのパッケージっぽい。<br>
bacula-clientはコンソールクライアントだけかな？それともfdとかsdだけ？<br>
（bacula-sqliteにもbconsoleが入っている）<br>
<br>
weeklyフル＋dailyインクリメンタルで設定したが、いまのところちゃんと動いているようだ。<br>
<br>
<br>
不満点<br>
・PoolにもJobDefsみたいなテンプレートが欲しい<br>
・bconsoleが使いにくい（ヒストリーとか）<br>
・batが不安定<br>
・バックアップのレシピ／ノウハウ集みたいなのが欲しい（こういうサイクルなら設定はこう書け！みたいな）。リテンション期間とかちゃんと考えるのがめんどい…<br>
・volume管理周りの用語集が欲しい<br>


