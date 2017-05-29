---
layout: post
title: "CentOS 5.3でyum-cronが無くなった"
date: 2009-05-13 13:13
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172441.html
---

ここんところ忙しくて書いていませんでした。<br>
この間にあったことはそのうち。<br>
<br>
さて、先ほどCentOS 5.3 をインストールして、各種必要なパッケージを追加していたのですが、<br>
いつもインストールしているyum-cronが、リポジトリからなくなっていることに気が付きました。<br>
<br>
調べてみると<br>
<br>
<blockquote><a href="http://www.bluequartz.us/phpBB2/viewtopic.php?p=315526&sid=451bf8b935d3124bce23b1381868ddd4">http://www.bluequartz.us/phpBB2/viewtopic.php?p=315526&sid=451bf8b935d3124bce23b1381868ddd4</a><br>
<br>
The reason why we carried it in 5.0 etc was since yum-updatesd didnt <br>
really do much. If there is anyway to make yum-updatesd run its updates <br>
at a specific time, that would completely remove the need / role for <br>
yum-cron. Which might be the best result allaround anyway.</blockquote><br>
「yum-cronを5.0で導入したのは、yum-updatesdがちゃんと動かなかったから。<br>
yum-updatesdで決まった時間にアップデートする方法が何かあるのなら、<br>
yum-cronの必要性／役割は完全になくなる。<br>
そいつはどっからどう見てもいいことだろ。」（訳適当）<br>
ってことらしい。<br>
<br>
でも、yum-updatesdで at a specific time ってできるのか？？？<br>
設定ファイルを見る限り、インターバルは決められても時間指定はできないっぽい。<br>
きっと、思想としては「チェック＆ダウンロードは自動で頻繁に、インストールタイミングは自分で選べ」ってことなんだろう。<br>
（でもそんなにクリティカルなサーバーじゃないので、夜間に自動インストールでいいんだけど）<br>
<br>
対処としては、<br>
１．あきらめてyum-updatesd使う<br>
２．他の人がやっているようにextraにあるyum-cronを使う<br>
３．自分でcron.dailyとかに書く<br>
になるか。<br>
<br>
標準以外の設定増えると面倒だし、１．を試すか…<br>


