---
layout: post
title: "Macでiconを差し替える"
date: 2012-11-26 15:19
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172482.html
---

同じアプリ(eclipseとかxcodeとか)を複数インストールする場合などに、アイコンを加工したいことがある。その場合の手順。<br>
<br>
1. Hoge.app/Contents/Resources にあるアイコン(icns)をプレビューで開き、tiffで書き出す。<br>
2. 適当に書き換える。ImageMagickならこんな感じ<br>
> convert Xcode.tiff -fill red -pointsize 256 -draw "text 600,900 '4.4'" -resize 256x256 Xcode_4_4.tiff<br>
3. tiff2icns でicnsに変換してから書き戻す（バックアップとってから）<br>


