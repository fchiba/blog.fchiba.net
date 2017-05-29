---
layout: post
title: "URL(URI)におけるqueryとparameterの違い"
date: 2009-06-24 15:06
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172447.html
---

http://www.ietf.org/rfc/rfc2396.txt<br>
によると、<br>
<blockquote>&lt;scheme>://&lt;authority>&lt;path>?&lt;query><br>
<br>
path          = [ abs_path | opaque_part ]<br>
path_segments = segment *( "/" segment )<br>
segment       = *pchar *( ";" param )<br>
param         = *pchar<br>
pchar         = unreserved | escaped | ":" | "@" | "&" | "=" | "+" | "$" | ","<br>
</blockquote><br>
ということらしい。<br>
<br>
つまり、; の後が parameter、? の後がqueryとなる。<br>
しかも、parameterはsegment毎に複数かける。ということは、<br>
<br>
http://example.com/dir1;a=1/dir2;b=2;c=3/index.html;d=4?f=5<br>
<br>
なんてのもありってことだな。<br>
ちゃんと解釈してくれるサーバーやフレームワークなんてなさそうだけど…。<br>
<br>
実際問題、parameterなんて使うのはJavaのjsessionidぐらいしか見たことないんだが、<br>
こいつのせいでいつも手間が増えるんだよなぁ。<br>


