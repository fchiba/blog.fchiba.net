---
layout: post
title: "ActionScript1.0のif文の展開"
date: 2011-03-09 17:54
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172476.html
---

知っている人には有名な話（自分用メモ）。<br>
<br>
ActionScript1.0(Flash Lite 1.1)でif文を書くとき、&amp;&amp;や||をつかうと、非常にいけてないバイトコードに展開される。<br>
例えば A &amp;&amp; Bとした場合、「Aが真じゃないとBは評価さえされない」というのを実現したかったんだと思うんだが、もう少しなんとかして欲しい…<br>
<br>
<table border="1"><tr><th>ActionScript</th><th>バイトコード</th></tr><tr><td><pre>if(A) {<br>
	ST<br>
}</pre></td><td><pre>	push	A<br>
	not<br>
	if	goto X<br>
	ST<br>
X:	end</pre></td></tr><tr><td><pre>if(A && B) {<br>
	ST<br>
}</pre></td><td><pre>	push	A<br>
	push	A<br>
	not<br>
	if	goto X<br>
	pop<br>
	push	B<br>
X:	not<br>
	if	goto Y<br>
	ST<br>
Y:	end</pre></td></tr><tr><td><pre>if(A && B && C) {<br>
	ST<br>
}</pre></td><td><pre>	push	A<br>
	push	A<br>
	not<br>
	if	goto X<br>
	pop<br>
	push	B<br>
X:	push	A<br>
	push	A<br>
	not<br>
	if	goto Y<br>
	pop<br>
	push	B<br>
	not<br>
Y:	if	goto Z<br>
	pop<br>
	push	C<br>
	not<br>
	if	goto W<br>
	ST<br>
W:	end</pre></td></tr><tr><td><pre>if(A||B) {<br>
	ST<br>
}</pre></td><td><pre>	push	A<br>
	push	A<br>
	if	goto X<br>
	pop<br>
	push	B<br>
X:	not<br>
	if	goto Y<br>
	ST<br>
Y:	end</pre></td></tr><tr><td><pre>if(A || B || C) {<br>
	ST<br>
}</pre></td><td><pre>	push	A<br>
	push	A<br>
	if	goto X<br>
	pop<br>
	push	B<br>
X:	push	A<br>
	push	A<br>
	if	goto Y<br>
	pop<br>
	push	B<br>
Y:	if	goto Z:<br>
	pop<br>
	push	C<br>
Z:	not<br>
	if	goto W:<br>
	ST<br>
W:	end</pre></td></tr></table><br>
<br>
ちなみに、上記では条件式を簡単にAと書いているが、もしvar1==var2みたいな文だったら、Aの「すべて」の場所に<br>
<pre>	push	"var1"<br>
	getVariable<br>
	push	"var2"<br>
	getVariable<br>
	equals</pre>なんてコードが展開されてしまう。<br>


