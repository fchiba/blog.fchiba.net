---
layout: post
title: "Androidのレイアウト計算について"
date: 2010-05-17 18:37
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172464.html
---

レイアウト計算はViewのツリーを上から順にたどって行われる。同レベルの場合は、XML中で先に書かれたものが先。（描画も同様）<br>
<br>
計算は２つのフェーズに分かれる。前半は必要なサイズを親が子へ問い合せる(measure phase)。後半では、親が実際の場所・サイズを決めて、子へ通知する。<br>
<br>
<h2>前半</h2><br>
前半では、親が子のmeasure(int widthMeasureSpec, int heightMeasureSpec)を呼び出す。<br>
measure(int,int)はonMeasure(int widthMeasureSpec, int heightMeasureSpec)を呼び出す。<br>
measure(int,int)はfinalなので、計算方法をカスタマイズするときはonMeasureをオーバーライドする。<br>
<br>
measureSpecは親が子に「こんな大きさになって欲しい」という要望を伝えるための変数。<br>
上位２ビットにモード、下位３０ビットにピクセル値が含まれるintの値で、View.MeasureSpecで計算する。<br>
（ちなみに、メモリとメソッド呼び出し回数の節約のためか、内部実装ではこのようなビット演算が多用されている）<br>
モードは以下の３種。<br>
<ul><li>UNSPECIFIED(お好きなように＝ピクセル値指定なし)</li><li>EXACTLY(厳密に指定のピクセル値にすべし)</li><li>AT_MOST(最大でも指定のピクセル値にすべし)</li></ul>measureSpecは子のLayoutParamsなどを参考にして設定する。<br>
<br>
親は繰り返し measure(int, int)を呼ぶことができる。最初の呼び出しで、全体の大きさを取得して、その値から計算した値をあらためてmeasureSpecに指定するような処理が可能。<br>
<br>
<h2>後半</h2><br>
後半は前半で収集した情報を使って、配置を確定し、子のlayout(int left, int top, int right, int bottom) を呼び出す。当然、配置のアルゴリズムはViewGroup毎に異なるが、layoutもfinalなのでカスタマイズは、onLayout(boolean changed, int left, int top, int right, int bottom)をオーバーライドする。<br>


