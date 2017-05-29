---
layout: post
title: "Android版Chromeで、ホーム画面のショートカットが効かない件"
date: 2014-11-05 15:40
comments: 
categories: 
author: fumiya_chiba
permalink: archives/1012988773.html
---

Android版Chromeで、ちょっと前のバージョンから、ホーム画面に作成したショートカットをタップしても該当ページが開かない問題が発生しています（Chrome自体は起動する）。しかし、Chromeのレビューを見ると再現している人としていない人がいるようです。<br /><br />で、調べてみたところ、手近な端末だとXperiaホームでだけ発生しているっぽいです。<br />&nbsp;<br />■正常に起動するもの<br />Nexus7, Nexus5, Xperia Z2のdocomoホーム画面<br /><br />■起動しないもの<br />Xperia Z2のXperiaホーム<br /><br /><br />正常に起動する場合のlogcatはこんな感じ。<br /><br /><blockquote>11-05 15:10:13.066 I/ActivityManager( &nbsp;660): START u0 {act=android.intent.action.VIEW dat=http://m.yahoo.co.jp/ flg=0x10000000 pkg=com.android.chrome cmp=com.android.chrome/com.google.android.apps.chrome.Main bnds=[12,853][276,1152] (has extras)} from pid 1118<br />&nbsp;</blockquote>一方起動しない場合は、<br /><br /><blockquote>11-05 15:11:03.675 I/ActivityManager( 1051): START u0 {act=android.intent.action.VIEW dat=http://m.yahoo.co.jp/ flg=0x10200000 pkg=com.android.chrome cmp=com.android.chrome/com.google.android.apps.chrome.Main (has extras)} from pid 1695</blockquote>&nbsp;<br />で、&nbsp;FLAG_ACTIVITY_RESET_TASK_IF_NEEDEDが立っています。<br /><br />手動でamコマンドで実行しても同様の結果です。<br /><blockquote>adb shell "am start -d http://m.yahoo.co.jp/ -a android.intent.action.VIEW -f 0x10000000"</blockquote>→OK<br /><blockquote>adb shell "am start -d http://m.yahoo.co.jp/ -a android.intent.action.VIEW -f 0x10200000"</blockquote>→NG<br /><br /><br /><br />残念ですが、暫定的な解決策はXperiaホーム使わない、ですかね…<br /><br />

