---
layout: post
title: "[Android]Scrollerのfling"
date: 2010-05-24 15:06
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172469.html
---

スクロール後の位置を計算してくれる Scroller というクラスがある。<br>
スクロール中の軌道(ある時間にどの位置にあるか？)を計算する方法として、コンストラクタに Interpolator を指定できる。<br>
これは、Scroller#startScrollでスクロールを開始した場合にしか有効でなく、Scroller#fling では単純な２次関数になる（等加速運動）。<br>
<br>
よく考えれば当たり前か。終了位置がわからないとInterpolatorも設定できないし。<br>
flingしたら、壁でbounceするアニメーションを実装したかったんだが、どうしよう？？<br>
<br>
<s>またコピペか？！</s><br>
コピペしないで済ませる方法を思いついた。初速から適当に終点を求めてそれでstartScrollすればよさそう。<br>


