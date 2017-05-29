---
layout: post
title: "ループバックNAT（ヘアピンNAT）をiptable-save/restoreに対応する形で書く"
date: 2015-08-09 01:51
comments: 
categories: 
author: fumiya_chiba
permalink: archives/1036481134.html
---

引き続き、linuxでルーターを作る話。<br /><br />IPマスカレードしているときに、外部に対してNATでサーバーを公開する場合、iptablesのnatテーブルはこんな感じになると思います。<br /><br />-A POSTROUTING -o wan -j MASQUERADE<br />-A PREROUTING -i wan -p tcp --dport 8080 -j DNAT --to 192.168.0.100:80<br /><br />そして、そのサーバーへLANの外からも中からも同じようにアクセスしたいとします。<br /><br />通常は、DDNSなどでルーターのグルーバルアドレスをサーバー名にひも付けていると思いますので、LAN側からだとルーターそのものへアクセスしてしまいます。<br /><br />一つの方法としては、LANの中でDNSを立ててLANからアクセスした際にはローカルのアドレスを返すというものです。これでも大体の場合はうまくいくのですが、上記のように外と中で使っているポートが違う（WANの8080をサーバーの80に転送している）ときは、うまくいきません。<br /><br />これをよしなにパケット転送して解決するのがループバックNAT（ヘアピンNAT）です。<br /><br />WAN側のアドレスがわかっている場合、書き方は簡単で、<br /><br />-A POSTROUTING -o wan -j MASQUERADE<br />-A PREROUTING -i wan -p tcp --dport 8080 -j DNAT --to 192.168.0.100:80<br />-A POSTROUTING -o lan -s 192.168.0.0/24 -d 192.168.0.100/32 -p tcp --dport 80 -j MASQUERADE<br />-A PREROUTING -i lan -p tcp -d (WANアドレス) --dport 8080 -j DNAT --to 192.168.0.100:80<br /><br />外からきた場合と同じように、中からWAN側アドレスに対してアクセスした場合にはマスカレードするだけです（ここまではググれば出てくる話）。<br /><br />しかし、これをサーバー上の設定ファイルに書こうと思うと困ったことがあります。それはWAN側のアドレスが変わってしまうということです。iptable-save/restoreは、マクロを展開するような機能を持っていないようですし、そもそもグローバルアドレスが変わるたびに実行しなおす仕組みが必要です。<br /><br />WAN側アドレスを汎用的に指定する方法がないか探していたところみつけたのが、addrtype拡張のLOCALアドレスタイプです。これはルーティングテーブル上ローカルに配送されるものが含まれるようです。（詳細は<a  target="_blank" href="http://linuxjm.osdn.jp/html/iptables/man8/iptables-extensions.8.html">man</a>を参照）<br /><br />これを使えば、最後の部分は<br /><br />-A PREROUTING -i lan -p tcp -m addrtype --dst-type LOCAL --dport 8080 -j DNAT --to 192.168.0.100:80<br /><br />と書けて無事解決です（ルーターのLAN側アドレスを除外しないと厳密には同じではないですが）。<br />

