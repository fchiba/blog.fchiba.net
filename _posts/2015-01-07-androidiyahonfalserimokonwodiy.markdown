---
layout: post
title: "AndroidイヤホンのリモコンをDIY"
date: 2015-01-07 00:19
comments: 
categories: 
author: fumiya_chiba
permalink: archives/1017084352.html
---

AndroidをカーオーディオのAUXに接続して、車の運転中にpodcastなどを聞いています。カーオーディオと違ってAndroidにはハードウェアキーが無いので、運転中に番組のスキップや早送りなどができなくて困っていました。<br /><br />「マイク付きヘッドホンアダプター」という商品名で、任意のイヤホンにマイク＆リモコンを後付けできる製品（たとえば&nbsp;<a  href="http://www.amazon.co.jp/gp/product/B00827LH46/ref=as_li_ss_tl?ie=UTF8&amp;camp=247&amp;creative=7399&amp;creativeASIN=B00827LH46&amp;linkCode=as2&amp;tag=fchiba-22">audio-technica AT338iS</a><img  style="border:none !important; margin:0px !important;" border="0" height="1" width="1" src="http://ir-jp.amazon-adsystem.com/e/ir?t=fchiba-22&amp;l=as2&amp;o=9&amp;a=B00827LH46">&nbsp;や&nbsp;<a  href="http://www.amazon.co.jp/gp/product/B0056XO4ZI/ref=as_li_ss_tl?ie=UTF8&amp;camp=247&amp;creative=7399&amp;creativeASIN=B0056XO4ZI&amp;linkCode=as2&amp;tag=fchiba-22">ELECOM MPA-IP353M3</a><img  src="http://ir-jp.amazon-adsystem.com/e/ir?t=fchiba-22&amp;l=as2&amp;o=9&amp;a=B0056XO4ZI" width="1" height="1" border="0" style="border:none !important; margin:0px !important;">&nbsp;など）が売られているのですが、<a  target="_blank" href="https://source.android.com/accessories/headset-spec.html">https://source.android.com/accessories/headset-spec.html</a>&nbsp;を見る限りそれほど大げさな構造ではなさそうなので自作してみることにしました。<br /><br />とりあえずボタンは１つだけあればよい（ボリュームはカーオーディオで操作できるしvoice searchなんて使わない）と割り切ります。リモコン付きマイクとして認識されるためには、マイク端子に、<br />・ボタンを押していない状態で1kΩ以上（マイクのインピーダンス。試したところ5kΩあれば安定）<br />・ボタンを押している状態で0Ω<br />の抵抗があればよいそうなので、抵抗とタクトスイッチをユニバーサル基盤の切れ端にはんだ付けしてリモコン部分は完了。<br /><br /><a  target="_blank" title="DSC_0413" href="http://livedoor.blogimg.jp/fumiya_chiba/imgs/8/5/8571d649.jpg"><img  class="pict" hspace="5" alt="DSC_0413" border="0" height="270" width="480" src="http://livedoor.blogimg.jp/fumiya_chiba/imgs/8/5/8571d649-s.jpg"></a><br /><br /><br />これを、そのへんに転がっていた、3.5mm 4pinジャック～RCA端子のケーブル（Xboxの付属品）に接続すれば完成。<br /><br /><a  target="_blank" title="DSC_0414" href="http://livedoor.blogimg.jp/fumiya_chiba/imgs/2/0/2080d1cb.jpg"><img  class="pict" hspace="5" alt="DSC_0414" border="0" height="270" width="480" src="http://livedoor.blogimg.jp/fumiya_chiba/imgs/2/0/2080d1cb-s.jpg"><br /></a><br />（このXboxのケーブル、幸いなことにピン配置がCTIAというスマートフォンで使われている規格と同じで根元から２番目がグラウンドになっていました）<br /><br />基盤むき出しがダサいですが、とりあえずしばらくこれで運用してみたいと思います。<br /><br /><br />

