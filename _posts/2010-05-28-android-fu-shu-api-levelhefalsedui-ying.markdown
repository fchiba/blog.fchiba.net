---
layout: post
title: "[Android]複数API Levelへの対応"
date: 2010-05-28 18:45
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172470.html
---

Androidで上位APIが使えるなら使い、使えないなら別な方法で対応する、というコードをどうやって書けばいいか検討してみた。<br>
<br>
例えば、画像の拡大縮小を<br>
・Ver2.0(API Level 5)以上でマルチタッチ対応なら、ピンチで行う。<br>
・上記以外の場合は、ZoomControlのみで行う。<br>
としたいとする。<br>
<br>
Eclipseのプロジェクト作成時、API Levelは1つしか選べない（チェックボックスなのに）。<br>
なので、上位のAPIを使いたい場合は、そのLevelに合わせる必要がある。<br>
ただし、下位の端末でも該当するAPIを使っていなければそのアプリケーションは実行可能である。<br>
(http://developer.android.com/intl/ja/guide/appendix/api-levels.html によると、"If the application were to be somehow installed on a platform with a lower API Level, then it would crash at run-time when it tried to access APIs that don't exist.")<br>
<br>
"tried to access APIs"が何を意味するのか実験してみたところ、上位APIを呼び出す前直前ではなく「上位APIを呼び出すコードを含んだクラスをロードする時点」（※）でクラッシュ(java.lang.VerifyError)してしまった。<br>
<br>
※…Class.forNameでの明示的ロード、インスタンス化、スタティックメソッド呼び出しなどのタイミング。<br>
<br>
ただし、リフレクションでクラスの情報を読み取るのは問題ない。<br>
<br>
ということで、<br>
・上位APIにアクセスする部分を他のクラスに追い出し、<br>
・リフレクションで上位APIの有無を確認し、<br>
・存在すればそのクラスをインスタンス化orスタティックメソッドで呼び出し、実際の処理を行う<br>
という手順を踏めばとりあえず実現できそう。<br>
<br>
こんなトリッキーな方法よりましな手段がありそうだが、現時点ではみつけられず。<br>


