---
layout: post
title: "Androidの外付けキーボードをカスタマイズする"
date: 2015-01-13 14:53
comments: 
categories: 
author: fumiya_chiba
permalink: archives/1017532431.html
---

<a  href="http://fchiba.blog.jp/archives/1017084352.html" target="_blank">前の記事</a>でリモコンを自作したのですが、２通りの操作しかできない（シングルクリックとダブルクリックのみ。長押しは音声検索に奪われてユーザには使えない）ので、結局<a  href="http://www.amazon.co.jp/gp/product/B008DM3V6W/ref=as_li_ss_tl?ie=UTF8&amp;camp=247&amp;creative=7399&amp;creativeASIN=B008DM3V6W&amp;linkCode=as2&amp;tag=fchiba-22">センチュリー Bluetooth接続マルチメディアリモコン iRemote Shutter</a><img  style="border:none !important; margin:0px !important;" border="0" height="1" width="1" src="http://ir-jp.amazon-adsystem.com/e/ir?t=fchiba-22&amp;l=as2&amp;o=9&amp;a=B008DM3V6W">を買ってしまいました。これはiPhone用ですが、Bluetoothキーボードとして認識されるらしいので、Androidでも使えることを期待して購入しました。<br /><br />実際に使ってみると、接続はスムーズにでき、再生・曲送り・ボリューム調整は問題なく動きました。ただし、接続中はソフトウェアキーボードが消えてしまうので、日本語入力はできません。ソフトウェアキーボードを出すボタンがついているのですが、Androidでは動作しませんでした（どうせ運転中にしか使わないので影響はありませんが）。<br /><br />車の運転中はボリューム調整がいらない（カーオーディオに接続しているのでそちらで調整できる）代わりに、早送り＆巻き戻しが欲しかったので、キーボードのレイアウトを調整することにしました。幸いにもAndroid4.1からはユーザー定義のキーボードレイアウトをインストールできるので、さくっと自作。これでだいぶ操作しやすくなった気がします。<br /><br />キーボードレイアウトのソースは<a  target="_blank" href="https://github.com/fchiba/iremoteshutter">github</a>にあげました。<br /><br /><hr>参考資料は以下の通りです。<br /><br /><div>Android入力デバイスの公式資料（ただし、カスタマイズ用ではなくデバイスメーカー向け）</div><div><a  href="http://source.android.com/devices/input/index.html" target="_blank">http://source.android.com/devices/input/index.html</a></div><div>Android入力デバイスの仕組みがわかりやすく解説されているマイナビの記事</div><div><a  href="http://news.mynavi.jp/column/androidnow/024/" target="_blank">http://news.mynavi.jp/column/androidnow/024/</a></div><div><a  href="http://news.mynavi.jp/column/androidnow/025/" target="_blank">http://news.mynavi.jp/column/androidnow/025/</a></div><div><a  href="http://news.mynavi.jp/column/androidnow/026/" target="_blank">http://news.mynavi.jp/column/androidnow/026/</a></div><div>カスタマイズキーボードを公開している方々</div><div><a  href="https://github.com/y10g/android_user-keymap_jp109keyboard" target="_blank">https://github.com/y10g/android_user-keymap_jp109keyboard</a></div><div><a  href="https://github.com/tialaramex/BritishKeyboard" target="_blank">https://github.com/tialaramex/BritishKeyboard</a></div><div>キーイベントの一覧</div><div><a  href="http://developer.android.com/reference/android/view/KeyEvent.html" target="_blank">http://developer.android.com/reference/android/view/KeyEvent.html&nbsp;</a></div>
<br />
<hr>
<br />このカスタムキーボードの技術的に興味深い点としては、<a  target="_blank" href="https://github.com/fchiba/iremoteshutter/blob/master/res/raw/iremoteshutter2.kcm#L4">https://github.com/fchiba/iremoteshutter/blob/master/res/raw/iremoteshutter2.kcm#L4</a>
のように、<br /><blockquote>map key (linuxのキーコード) (Androidのキーコード)
</blockquote>とかけば、kcm(=Key Character Map)という本来は文字の変換しか出来ないファイルにも関わらずキー自体の配置を変えられてしまうところです。これはy10gさんのソースを見ていて発見したんですが、undocumentedな仕様ですかね…

