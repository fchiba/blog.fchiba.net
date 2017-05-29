---
layout: post
title: "AndroidのSurfaceViewのSurfaceのライフサイクル"
date: 2013-09-05 17:12
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172485.html
---

他のアプリ起動→復帰<br>
destroy→create/change<br>
<br>
端末サスペンド→復帰<br>
なにもなし→なにもなし（生きたまま）<br>
<br>
invisibleにする→visibleにする<br>
destroy→create/change<br>
<br>
Orientation変更(configChanges指定なし)<br>
destroy→create/change<br>
<br>
Orientation変更(configChanges指定あり)<br>
changedのみ<br>
<br>
Orientation変更中に別ThreadからさらにsetRequestedOrientation呼び出し（詳細条件不明）<br>
予期せぬdestroy発生！<br>


