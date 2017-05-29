---
layout: post
title: "Raspberry Piでエアコンを操作できる赤外線リモコンを作った"
date: 2013-08-11 18:11
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172484.html
---

Raspberry Piでエアコンを操作できる赤外線リモコンを作りました。類似の事例はネット上に数多く見つかるのですが、「エアコン以外の機器を操作している」もしくは「Raspberry Pi以外で実現している」ケースしかみつからなかったので、記録に残しておきます。<br>
<br>
主に参考にしたのは以下のサイトです。<br>
<a href="http://homebrew.jp/show?page=1480" target="_blank" title="http://homebrew.jp/show?page=1480">http://homebrew.jp/show?page=1480</a><br>
<a href="http://www.256byte.com/remocon.htm" target="_blank" title="http://www.256byte.com/remocon.htm">http://www.256byte.com/remocon.htm</a><br>
<a href="http://www.geocities.jp/bokunimowakaru/std-commander.html" target="_blank" title="http://www.geocities.jp/bokunimowakaru/std-commander.html">http://www.geocities.jp/bokunimowakaru/std-commander.html</a><br>
<br>
最初のサイトは「エアコン以外の機器を操作している」ケースで、この通りにやったところ、実際にテレビや照明などの操作はうまくいきましたので、これを前提に行います。<br>
<br>
うちのエアコン（ダイキン製）で試した際に躓いたのは、irrecordで信号を記録させるところです。ちょっと調べた方はすぐにわかると思うのですが、エアコンは送信すべき情報量が多く信号のフォーマットが規格とは若干違うのが原因のようです。<br>
<br>
そこで、mode2コマンドで信号のパルス列を調べてみることにしました。mode2の出力は<br>
<blockquote>space 2309756<br>
pulse 465<br>
space 401<br>
pulse 471<br>
space 392<br>
pulse 446<br>
space 438<br>
pulse 451<br>
space 398<br>
pulse 468<br>
space 410<br>
pulse 433<br>
space 25278<br>
pulse 3490<br>
space 1703<br>
pulse 464<br>
space 1264</blockquote><br>
のような形式です。信号のフォーマットに関しては上記の２番目のサイトがわかりやすかったです。見たところ、点灯している時間がほぼ一定で、消灯している時間に差があるため、NECフォーマットを拡張したもののようです。そこで、こちら <a href="https://gist.github.com/fchiba/6204022" target="_blank" title="https://gist.github.com/fchiba/6204022">https://gist.github.com/fchiba/6204022</a> のようなプログラムを書いて、でもう少し人間に読みやすいような形式にして、詳しく覗いてみることにしました。<br>
<br>
<br>
通常のNECフォーマットでは、１つの操作は、<br>
<br>
リーダー信号＋データ信号＋ストップ信号<br>
<br>
で成り立ちます。つまり、リモコンで１回ボタンを押してすぐ離すと、これが１回送信されます。しかし、ダイキンのエアコンでは、１回のボタン操作で<br>
<br>
1) "bit 0"×6回＋ストップ信号<br>
2) リーダー信号＋データ信号(64bit)＋ストップ信号<br>
3) リーダー信号＋データ信号(64bit)＋ストップ信号<br>
4) リーダー信号＋データ信号(152bit)＋ストップ信号<br>
<br>
がまとめて送信されます。最初の1)〜3)までは常に一定で、温度や風量などを変更した際には4)の152bitの部分だけが変わるようです。<br>
<br>
この152bitのデータ信号の部分を細かく解析していくと、本物のリモコンと同じようなことができそうです。しかし、今回はプリセットでいくつかの定型的な操作さえできればいいので、上記のmode2の結果を編集して、lircd.confにraw形式で貼り付けておしまいにしました（mode2をそのまま再生できる方法があればよかったのですが…）。具体的には <a href="https://gist.github.com/fchiba/6204072" target="_blank" title="https://gist.github.com/fchiba/6204072">https://gist.github.com/fchiba/6204072</a> の２つ目のremoteセクションのような感じです。<br>
<br>
ちなみに、信号の記録の際には、何度か試して正しくパースできるものを選んでいます。ノイズなどがあると、例えばストップ信号が途切れ途切れになったり、bit1が正しく1:3にならず1:2になったりします。こういう場合は、カーテンを閉めテレビなどを消すと、ノイズは減りました。<br>
<br>
あと、信号が弱かったため３番目のサイトを参考にコンデンサを挟んでやったところ、実用的な強度で光ってくれるようになりました。<br>


