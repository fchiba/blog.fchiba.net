---
layout: post
title: "Amazon EC2 と Google App Engine のネットワーク速度"
date: 2009-07-24 21:20
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172449.html
---

Amazon EC2とGoogle App Engineについて、転送量に関するスペックは公開されているものの、ネットワーク帯域に関する情報はみつからなかったので測ってみた。<br>
<br>
計測にはapache benchを使用。いずれも約70KByteのデータをHTTPでダウンロード。<br>
クライアントは日本国内からフレッツ光で接続。<br>
条件をちゃんとそろえているわけではないのであくまでも参考値。<br>
<br>
・Amazon EC2 (Small Instance1台)<br>
レスポンス　1.5s<br>
スループット　＞700KB/s（？）<br>
<br>
さすがに海の向こうなのでレスポンスは遅い。（PC向けだとちょっと気になるが、携帯電話向けサービスなら我慢の範囲内）<br>
しかし、コンカレンシーをあげるとスループットはそれなりに伸びる。<br>
コンカレンシー６０あたりで伸びがゆるくなったが、クライアントに先に限界が来たか？？？<br>
<br>
・Google App Engine<br>
レスポンス 0.5s<br>
スループット  2000KB/s<br>
<br>
レスポンスは優秀。きっと日本国内にデータセンターがあるのだろう。<br>
abでコンカレンシーを増やしても、2000KB/sで頭打ちに。<br>
転送量は金払えば増えるけど、帯域はどうなんだろ。<br>
<br>
（参考）<br>
・さくら専用サーバーエントリー（10M共有回線）<br>
レスポンス　0.5s<br>
スループット　1000KB/s<br>


