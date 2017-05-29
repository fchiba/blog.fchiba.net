---
layout: post
title: "AndroidのMatrix"
date: 2010-05-17 19:04
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172465.html
---

<a href="http://developer.android.com/intl/ja/reference/android/graphics/Matrix.html" target="_blank" title="Matrix">Matrix</a>について。<br>
<br>
Flashと同じで、変形を<br>
<br>
<table border="0" cellpadding="1" cellspacing="5"><tbody></tbody><tbody><tr><td></td><td>X</td><td></td><td>ScaleX</td><td>SkewX</td><td>TranslateX</td><td></td><td>x</td><td></td></tr><tr><td>(</td><td>Y</td><td>)=(</td><td>SkewY</td><td>ScaleY</td><td>TranslateY</td><td>)(</td><td>y</td><td>)</td></tr><tr><td></td><td>1</td><td></td><td>0</td><td>0</td><td>1</td><td></td><td>1</td><td></td></tr></tbody></table><br>
<br>
という行列で表す。<br>
<br>
メソッド名でpostとかpreとかあるのは、元の行列の前に掛けるか後に掛けるかを表している。<br>
主にtranslateが影響を受ける(はず)。<br>


