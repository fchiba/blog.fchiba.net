---
layout: post
title: "[Android]TextViewにリンクを設定して他のActivityを設定する方法"
date: 2010-06-09 19:32
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172471.html
---

TextViewに表示されているテキストからURL、@user、#hashなどの文字列を抽出してリンクにし、他のActivityを起動したい。（いわゆるTwitterクライアント）<br>
<br>
この辺を参考に（特に２つ目）した。<br>
http://developer.android.com/intl/ja/resources/articles/wikinotes-linkify.html<br>
http://developer.android.com/intl/ja/resources/articles/wikinotes-intents.html<br>
http://developer.android.com/intl/ja/guide/topics/intents/intents-filters.html<br>
<br>
1. リンク作成<br>
まず、Linkifyクラスで、該当の文字列を抜き出してリンクにする。<br>
httpで始まる単純なURLは、そのままの文字列で暗黙のIntentを発行。<br>
@userや#hashなどは、Linkify.TransformFilterを使って、content://で始まるURIに変換する。<br>
URIのサーバー名に当たる部分(Authorityという)は、パッケージ名などを元にユニークな名前にする。<br>
　例：net.fchiba.sampleapp など。<br>
パスに当たる部分は、アプリごとに適当に設計する(RESTな感じで)。<br>
　例： /user/(id) とか。<br>
<br>
2. ContentResolverの作成<br>
URIからMIMEタイプを求めるクラスを作成する。ContentResolverを継承したクラスを作成し、getTypeをオーバーロードする。<a href="http://code.google.com/p/apps-for-android/source/browse/trunk/WikiNotes/src/com/google/android/wikinotes/db/WikiNotesProvider.java" target="_blank" title="サンプル">サンプル</a>を参考にして、getTypeのみ実装。<br>
MIMEタイプは、image/jpegなど決められているもの以外の場合、vndで始まりユニークになるように決める。<br>
　例： @id なら vnd.net.fchiba.sampleapp.twitter/user、<br>
　　　　#hash なら vnd.net.fchiba.sampleapp.twitter/search とか<br>
作ったクラスをAndroidManifest.xmlに登録。<br>
　&lt;provider android:name="(クラス名)" android:authorities="net.fchiba.sampleapps" /&gt;<br>
<br>
3. 処理を受け取るActivityを作成<br>
1.で発行されるのは暗黙のintentなので、intent-filterで受け取る必要がある。<br>
発行されるActionは android.intent.action.VIEW。2.で決めたMIMEタイプで受け取るActivityを振り分ける。<br>
ユーザーを処理する Activityなら、<br>
　&lt;intent-filter><br>
　　&lt;action android:name="android.intent.action.VIEW" /><br>
　　&lt;category android:name="android.intent.category.DEFAULT" /><br>
　　&lt;data android:mimeType="vnd.net.fchiba.sampleapp.twitter/user" /><br>
　&lt;/intent-filter><br>
など。<br>
<br>
<br>
TODO<br>
・ユーザー名やハッシュタグを抜き出す正規表現を洗練させる。<br>
・リンククリック時にメニューなどを表示させる方法を調べる。シングルクリックでデフォルトアクション、ロングクリックでメニュー選択がいいよね。android.intent.category.ALTERNATIVEなどが怪しげ。<br>


