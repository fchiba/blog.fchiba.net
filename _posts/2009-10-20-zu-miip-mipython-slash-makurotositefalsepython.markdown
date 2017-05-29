---
layout: post
title: "組み込みPython/マクロとしてのPython"
date: 2009-10-20 21:29
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172457.html
---

現在、C++で作っているサーバーソフトをpure Javaな環境で動かす必要が出てきた。<br>
できればJNIなどは使わないほうがよいとのこと。<br>
<br>
そっくり移植するという手もあるが、似たようなコードを2重メンテするのは手間。<br>
ということで、外部とのインターフェースはネイティブコードで書いて、ロジックの複雑なコア部分を他の言語で書くことにした。<br>
処理速度の都合上、末端の処理はやはりネイティブ言語で書く必要があったため、<br>
　　外部インターフェース → コア(複雑なロジック) → 末端の重いけど単純な処理<br>
　　　　　　C++　　　　　　　　　組み込み言語（共通）　　　　C++<br>
　　　　　　Java　　　　　　　　 組み込み言語（共通）　　　　Java<br>
というマクロ言語的な使い方となる。<br>
<br>
当初はLuaを考えていたが LuaJavaが外部ライブラリを使う仕様となっていたため断念。<br>
他に、上記のようにネイティブ→組み込み、組み込み→ネイティブの呼び出しができるのは、Pythonぐらいしかみつからなかった。Pythonは今まで使ったことがなかったが、ちょうどいい機会なので覚えてしまうことにする。<br>
<br>
Pythonから末端のC++の呼び出しのためにラッパーが必要となるが、今回は SWIG を使用。<br>
（boost::pythonやCythonなども検討したが、いろいろあって見送り。）<br>
組み込みの方法は<br>
<a href="http://lahosken.san-francisco.ca.us/frivolity/prog/embswig/doc.html" target="_blank" title="http://lahosken.san-francisco.ca.us/frivolity/prog/embswig/doc.html">http://lahosken.san-francisco.ca.us/frivolity/prog/embswig/doc.html</a><br>
が参考になった。<br>
<br>
しかし、上記の方法ではPythonのプロキシクラスを毎回ディスクから直接ロードすることになる。<br>
これが嫌だったので、<br>
<a href="http://fchiba.blog114.fc2.com/blog-entry-23.html" target="_blank" title="http://fchiba.blog114.fc2.com/blog-entry-23.html">http://fchiba.blog114.fc2.com/blog-entry-23.html</a><br>
により、ソース中にプロキシクラスを埋め込むことにした。<br>
<br>
また、Python→Javaの呼び出し時にはパッケージ名を指定してimportするが、<br>
Pythonソースを共通化するためには、C++も同じパッケージにする必要がある。<br>
そこで、<br>
<a href="http://fchiba.blog114.fc2.com/blog-entry-24.html" target="_blank" title="http://fchiba.blog114.fc2.com/blog-entry-24.html">http://fchiba.blog114.fc2.com/blog-entry-24.html</a><br>
で、SWIGで生成したコードにパッケージ名をつけた。<br>
<br>
とりあえず簡単なコードで上記の実証実験が行えたので、これから実際のソースに適用してみることにする。<br>


