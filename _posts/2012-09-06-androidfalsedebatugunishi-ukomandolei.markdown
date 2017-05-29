---
layout: post
title: "Androidのデバッグに使うコマンド類"
date: 2012-09-06 16:35
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172480.html
---

アプリケーション管理（デフォルト）<br>
adb shell 'am start -a android.settings.MANAGE_APPLICATIONS_SETTINGS'<br>
<br>
アプリケーション管理（すべて＆サイズ順）<br>
adb shell 'am start -a android.intent.action.MANAGE_PACKAGE_STORAGE'<br>
<br>
アプリケーション管理＞詳細<br>
adb shell 'am start -a android.settings.APPLICATION_DETAILS_SETTINGS package:パッケージ名'<br>
<br>
パッケージ情報<br>
adb shell 'dumpsys package [パッケージ名]'<br>
<br>
メモリ消費量<br>
adb shell 'dumpsys meminfo [パッケージ名]'<br>


