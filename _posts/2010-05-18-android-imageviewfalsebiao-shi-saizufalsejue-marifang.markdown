---
layout: post
title: "[Android]ImageViewの表示サイズの決まり方"
date: 2010-05-18 16:51
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172467.html
---

ImageViewのビュー自体のサイズと、中身(Drawable)の表示のされ方はいろいろな要素に影響されて決まる。<br>
ちょっとわかりにくいのでざっと整理してみたいと思う。<br>
<br>
（この記事を読むには、<a href="http://fchiba.blog114.fc2.com/blog-entry-32.html" target="_blank" title="Androidのレイアウト計算について">Androidのレイアウト計算について</a>などで、レイアウトの決まり方を知っておく必要あり）<br>
<br>
サイズに影響を与える要素は以下の10個がある。<ol><br>
<li>親ViewGroupからのmeasureSpec<br>
<li>DrawableのIntrinsicWidth/Height（Bitmapなどの大きさ）<br>
<li>minWidth/Height<br>
<li>maxWidth/Height<br>
<li>BackgroundDrawableのIntrinsicWidth/Height<br>
<li>adjustViewBounds<br>
<li>padding<br>
<li>matrix<br>
<li>scaleType<br>
<li>cropToPadding<br>
</ol><br>
このうち、ビューのサイズに影響を与えるのは1～7のみで、8～10はDrawableの表示のみに影響を与える。<br>
<br>
ImageViewのソースから、Drawableをセットした場合の処理の流れを追いかけてみる。<br>
setImageURI/setImageResource/setImageDrawableなどDrawableを変更するメソッドを呼ぶと、最後にrequestLayoutが呼ばれる。<br>
requestLayoutを呼び出すと、フレームワークからmeasureが呼ばれ、measureの中からonMeasureが呼ばれる。ここでViewのWidth/Heightが決まる。<br>
Width/Heightが決まるとフレームワークからlayoutが呼ばれ、layoutの中からsetLayoutが呼ばれる。setLayoutはさらにconfigureBounds(private関数)を呼び出す。ここで、Drawableの表示方法が決められる。<br>
<br>
<h2>onMeasure</h2><br>
onMeasureの中では、以下のような方法で幅・高さが決められている（ただし、説明を簡単にするためにpaddingは0と仮定）<br>
・adjustViewBoundsがtrueで、measureSpecの指定が幅・高さのどちらかが可変の場合<br>
　DrawableのIntrinsicWidth/Heightを、measureSpecとmaxで調整して幅・高さを仮ぎめする（ここで最大が決まる）<br>
　幅・高さをDrawableのアスペクト比と同じになるように調整する。<br>
　順序は、幅→高さの順。仮ぎめしたものより小さくなる。<br>
　調整が効かなかった場合は、仮ぎめしたものがそのまま使われる。<br>
　<br>
・上記以外の場合<br>
　DrawableのIntrinsicWidth/Heightを、<br>
　・measureSpec/EXACTLY<br>
　・min以上 && BackgroundDrawableのIntrinsicWidth/Height以上<br>
　・max以下 && measureSpec/AT_MOST 以下<br>
　の制約条件を満たすように調整する（優先度は上から順）<br>
　<br>
(注)adjustViewBounds=trueの場合、minは無視されることになる<br>
<br>
<h2>configureBounds</h2><br>
configureBoundsでは、DrawableのIntrinsicWidth/HeightとViewのWidth/Heightが所与のものとして、ScaleTypeを元にDrawableを描画する時のMatrixを計算する。<br>
（レイアウトが完了していない場合にも呼ばれることもあるが、その場合は何もしない）<br>
<br>
・fitXY<br>
　Viewのwidth,heightになるようにMatrixを設定<br>
・matrix<br>
　setImageMatrixで指定したMatrixをそのまま使う<br>
・center<br>
　Drawableを中央に配置するようにMatrixを設定<br>
・center_crop<br>
　Drawableを、アスペクト比を維持しつつ隙間がなくなるように拡大・縮小して中央に配置するようにMatrixを設定<br>
・center_inside<br>
　Drawableを、アスペクト比を維持しつつ全てが収まるように拡大・縮小して配置するようにMatrixを設定<br>
・fitStart, fitEnd, fitCenter<br>
　Matrix#ScaleToFitにおまかせ<br>
<br>
<h2>onDraw</h2><br>
onDrawで計算したMatrixを使って描画を行う。<br>
ただし、ViewよりDrawableのほうが大きい場合、CropToPaddingでpaddingの処理が変わる。<br>
具体的には、CropToPaddingがfalse(デフォルト)の場合は、はみ出した箇所にはpaddingが表示されない(paddingの領域にもDrawableが描画される)が、trueだと常にpaddingが表示される。<br>


