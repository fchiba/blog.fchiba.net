---
layout: post
title: "Raspberry Piでルータを作ったらVPN(PPTP)が通らなかった件"
date: 2015-08-02 13:40
comments: 
categories: 
author: fumiya_chiba
permalink: archives/1035869359.html
---

Raspberry Piでルーターを作ったのだが、VPN(PPTP)が通らなかった。<br /><br />VPN Clientのログ(Macなので/var/log/ppp.log)を見たところ、LCP ConfReq というパケットを送ったのに、その返信が返ってこなくてタイムアウトしている模様。<br /><br />ググると、GREパケットが途中で落ちているとのことなので、tcpdumpで調査。ppp0を見ると返ってきているのに、eth1(LAN側)には返ってきていないので、NATの問題だと予想。さらにググった結果、<br />http://serverfault.com/questions/167485/pptp-gre-multi-forwarding-nat-iptables-example<br /><br />を発見し、<br />modprobe nf_conntrack_pptp<br />modprobe ip_nat_pptp<br />で解決。<br />&nbsp;

