---
layout: post
title: "tcpdumpでパケットキャプチャ"
date: 2010-02-10 17:28
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172463.html
---

（社内向け）<br>
<br>
サーバー間のAPI呼び出しなどをデバッグする際、パラメータやレスポンスをログに出すよりは、<br>
パケットキャプチャで実際の通信を確認するほうが手っ取り早いことが多いです。<br>
<br>
サーバーでキャプチャするには、tcpdump を使います。<br>
tcpdumpの実行には root 権限が必要なので、まず su - します。<br>
(tcpdump は /usr/sbin にあるので、su でなくsu - としたほうが楽）<br>
<br>
<strong>実行例１： tcpdump -X -s 1000 port 80 and host XXX.XXX.XXX.XXX</strong><br>
<br>
意味<br>
-X<br>
　コンソールに 16進表示＋ASCII文字で表示<br>
-s 1000<br>
　１パケットあたり1000バイトまでキャプチャ（デフォルトは68で少なすぎる）<br>
port 80 and host XXX.XXX.XXX.XXX <br>
　XXX.XXX.XXX.XXXのポート80番あての通信をキャプチャ（デフォルトではすべての通信をキャプチャ）<br>
<br>
<strong>実行例２：tcpdump -w test.pcap -s 1000 port 80 and host XXX.XXX.XXX.XXX</strong><br>
<br>
意味<br>
-w test.pcap<br>
　test.pcapにキャプチャした結果を保存<br>
　保存したファイルをPCにコピーすれば Ethereal で閲覧可能。こっちのほうが圧倒的に見やすいです。<br>
　<br>
Ethereal も有名なキャプチャソフトですが、単なるビュアーとして使うことも可能です。<br>
<a href="http://www.ethereal.com/download.html" target="_blank" title="http://www.ethereal.com/download.html">http://www.ethereal.com/download.html</a>からダウンロードして下さい。<br>


