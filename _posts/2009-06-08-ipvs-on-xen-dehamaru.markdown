---
layout: post
title: "ipvs on xen ではまる"
date: 2009-06-08 19:10
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172444.html
---

xen上に、ipvs(w/keepalived)でロードバランサを作ったが、再送が多発し速度がでない。<br>
<br>
どうやら原因は、TCP offloadとxenの問題らしい（http://lists.graemef.net/pipermail/lvs-users/2007-August/019639.html）。<br>
<br>
/sbin/ifup-local<br>
に<br>
<pre><br>
DEVICE="$1"<br>
case "$DEVICE" in<br>
eth0)<br>
ethtool -K $DEVICE tx off<br>
;;<br>
esac<br>
</pre><br>
と書いて解決。<br>


