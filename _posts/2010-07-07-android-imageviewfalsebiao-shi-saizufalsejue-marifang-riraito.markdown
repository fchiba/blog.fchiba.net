---
layout: post
title: "[Android]ImageViewの表示サイズの決まり方（リライト）"
date: 2010-07-07 13:02
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172472.html
---

＃ http://fchiba.blog114.fc2.com/blog-entry-35.html をもう少しわかりやすく書きなおした。<br>
＃ レイアウト計算の詳細を知らなくても読めるようになったはず。<br>
<br>
<br>
ImageViewの表示サイズは、以下の要素によって決まります。<br>
<br>
1. layout_width / layout_heightのパラメータ<br>
2. Drawableの画像サイズ(IntrinsicWidth/Height)<br>
3. minWidth / minHeight<br>
4. maxWidth / maxHeight<br>
5. BackgroundDrawable の画像サイズ(IntrinsicWidth/Height)<br>
6. adjustViewBounds<br>
7. padding<br>
8. matrix<br>
9. scaleType<br>
10. cropToPadding<br>
<br>
このうち、1.～7.によってImageViewの大きさが決まります。<br>
8.～10.は、ImageViewの大きさが所与のものとして、その中でDrawableがどのように描画されるかを決めるパラメータです。<br>
<br>
以下では話を簡単にするために、<br>
・親ViewGroupがLinearLayoutで、orientaion=vertical, layout_width=fill_parent, layout_height=fill_parent<br>
・ImageViewは、layout_width/layout_height に wrap_content か fill_parent のどちらかを指定（ピクセル指定しない）、padding=0<br>
を仮定しています。<br>
<br>
<br>
■View自体の大きさの決まり方<br>
<br>
A. adjustViewBounds = true かつ layout_width/layout_height のどちらかにwrap_content が指定されている場合、<br>
　A-1)　width/heightをDrawableのIntrinsicWidth/Heightで仮ぎめ<br>
　A-2)　layout_width/layout_heightに、fill_parentが指定されていれば、親Viewの値で上書き<br>
　A-3)　layout_width/layout_heightに、wrap_contentが指定されていれば、親Viewより小さくなるようにする<br>
　　　　（ここで決めた値以上になることはない）<br>
　A-4)　layout_width が wrap_content の場合、layout_heightを固定のまま、Drawableのアスペクト比とおなじになるように layout_width を変更。<br>
　　　　(変更の結果 layout_width が元の値より大きくなってしまう場合はそのまま)<br>
　A-5)　A-4) で調整ができず layout_height が wrap_content の場合、<br>
　　　　layout_widthを固定のまま、Drawableのアスペクト比とおなじになるように layout_height を変更。<br>
　　　　(変更の結果 layout_height が元の値より大きくなってしまう場合はそのまま)<br>
<br>
　注： A. の場合、minWidth / minHeight と BackgroundDrawableのIntrinsicWidth/Height は考慮されません。<br>
<br>
B. A.以外の場合、<br>
　B-1) fill_parent が指定されていれば、その値<br>
　B-2) fill_parent が指定されていない場合、Drawableのwidth/heightを下記の順で調整する<br>
　　　　(i) minWidth / minHeight 以上<br>
　　　　(ii) BackgroundDrawableのIntrinsicWidth/Height 以上<br>
　　　　(iii) maxWidth / maxHeight 以下<br>
<br>
<br>
■描画のされ方<br>
scaleTypeの値によってDrawableが表示される位置と拡大率が決まります。<br>
<br>
・fitXY<br>
　ImageViewの大きさにぴったりあうように表示<br>
・matrix<br>
　setImageMatrixで指定したMatrixをそのまま使う<br>
・center<br>
　Drawableを中央に配置する。拡大率は1（オリジナル画像のまま）<br>
・center_crop<br>
　Drawableを、アスペクト比を維持しつつ隙間がなくなるように拡大・縮小して中央に配置するように描画<br>
・center_inside<br>
　Drawableを、アスペクト比を維持しつつ全てが収まるように拡大・縮小して中央に配置するように描画<br>
・fitStart, fitEnd, fitCenter<br>
　Matrix.ScaleToFit(http://developer.android.com/intl/ja/reference/android/graphics/Matrix.ScaleToFit.html)におまかせ<br>
<br>
Drawableの描画がImageViewの大きさからはみ出す場合、cropToPaddingの値により、paddingの処理が変わります。<br>
具体的には、CropToPaddingがfalse(デフォルト)の場合は、はみ出した箇所にはpaddingが表示されません(paddingの領域にもDrawableが描画される)が、<br>
trueだと常にpaddingが表示されます。<br>


