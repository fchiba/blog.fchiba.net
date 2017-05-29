---
layout: post
title: "canvasのglobalCompositeOperationの計算式らしきもの"
date: 2011-05-10 17:01
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172477.html
---

（5/13追記あり）<br>
<br>
canvas(HTML5)のglobalCompositeOperationがどういう計算をしているか気になったので調べた。<br>
<br>
以下は、webkitのソースをGoogleコードサーチで検索した結果。<br>
<br>
globalCompositeOperationは<a href="http://www.google.co.jp/codesearch/p?hl=ja#N6Qhr5kJSgQ/WebCore/html/canvas/CanvasRenderingContext2D.cpp&q=setGlobalCompositeOperation&sa=N&cd=1&ct=rc">CanvasRenderingContext2D#setGlobalCompositeOperation</a>で実装されていると思われる。ここから<a href="http://www.google.co.jp/codesearch/p?hl=ja#N6Qhr5kJSgQ/WebCore/platform/graphics/GraphicsContext.h&q=setCompositeOperation&sa=N&cd=1&ct=rc">GraphicsContext#setCompositeOperation</a>が呼ばれているが、GraphicsContextは各プラットフォーム毎に実装されるものらしく#if PLATFORM(xxx)が沢山並ぶなんとも面倒そうな感じ。とりあえずAndroidで使われているskiaというライブラリを使う実装を見ると<a href="http://www.google.co.jp/codesearch/p?hl=ja#N6Qhr5kJSgQ/WebCore/platform/graphics/skia/SkiaUtils.cpp&q=gMapCompositOpsToSkiaModes&sa=N&cd=1&ct=rc">globalCompositeOperationの値とskiaの合成モードの変換表</a>とskiaの合成モードでの変換式を<a href="http://www.google.co.jp/codesearch/p?hl=ja#wB4xAW-hpIM/include/effects/SkPorterDuff.h&q=SkPorterDuff%20android&sa=N&cd=1&ct=rc">コメント中に発見</a>。<br>
<br>
きれいに整形したり、転載するのはめんどうなので、知りたい方は上記リンクを確認してください。<br>


