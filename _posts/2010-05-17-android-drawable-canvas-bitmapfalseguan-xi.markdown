---
layout: post
title: "[Android] Drawable, Canvas, Bitmapの関係"
date: 2010-05-17 19:39
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172466.html
---

Drawable, Canvas, Bitmapの関係が最初わからなかったので整理。<br>
<br>
Bitmapは、ピクセル(点)のデータの集まり。<br>
<br>
Drawableは、<a href="http://developer.android.com/intl/ja/reference/android/graphics/drawable/Drawable.html" target="_blank" title="http://developer.android.com/intl/ja/reference/android/graphics/drawable/Drawable.html">http://developer.android.com/intl/ja/reference/android/graphics/drawable/Drawable.html</a>に<br>
A Drawable is a general abstraction for "something that can be drawn."<br>
とある通り、「何を」描くかを表す抽象クラス。<br>
<br>
単純なBitmap以外にも、線・塗り、NinePatchなどがDrawableのサブクラスとして実装されている。Resourceと組み合わせて使うことが多い。<br>
<br>
また、Viewで使用するための機能として、ステータスやレベルなどがあり、１つのインスタンスに複数の状態を保持できる。StateListDrawableなどといった専用のクラスがあり、他のDrawableを保持することで実現している。<br>
ステータスは、"フォーカスが当たった"/"押された"/"無効"などがある。レベルは任意のint型の数値が指定可能（ボリュームコントロールやバッテリーメーターなどで使う）。<br>
<br>
<br>
Canvas は「描く」部分を集約したクラス。「線を描く」「Bitmapを流し込む」「テキストを描く」などのメソッドを持つ。どこに描くかはコンストラクタで指定する。<br>
<br>
Drawable#draw(canvas) で、Drawableの内容をCanvasに描き込むことができる。<br>


