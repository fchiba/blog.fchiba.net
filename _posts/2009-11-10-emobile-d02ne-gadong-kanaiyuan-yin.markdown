---
layout: post
title: "EMOBILE(D02NE)が動かない原因"
date: 2009-11-10 20:53
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172460.html
---

EMOBILE(D02NE)を使おうとしていたのだが、ドライバのインストール後に<br>
「データ端末の通信ポートが利用できませんでした」<br>
というダイアログがでて動かなくて困った。<br>
<br>
いろいろ試したが、おそらく原因はVMWare Server。<br>
<br>
カードをPCに挿すとき（ドライバがインストールされるとき）に、VMWareのサービスを落とすと動くようになった。いったん動くようになったら、VMWare Serverのサービスは立ち上げてOK。<br>
<br>
逆に、いったん上記ダイアログがでる状態になってしまうと、ドライバをアンインストールする以外に動作させる方法は見つからなかった。<br>
（「EMOBILE D02NEユーティリティ」と「Windows ドライバパッケージ - NEC Inforontia (d02nemdm) Modem」の２つをコントロールパネルで削除する）<br>
<br>
以前、D01NEが使えなくてあきらめたことがあるが、おそらくそちらも同じだと思われる。<br>


