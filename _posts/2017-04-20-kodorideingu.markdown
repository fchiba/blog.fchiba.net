---
layout: post
title: "コードリーディング"
date: 2017-04-20 18:06
comments: 
categories: 
author: fumiya_chiba
permalink: archives/1065609856.html
---

そこそこ複雑なコードを読む場合に、紙に印刷して読むことがあります。<br /><br />例えば、１ファイル数千行ぐらいのあまり構造化されてないpythonスクリプトの処理内容を把握したいときなど。<br /><br />紙に印刷するメリットは、<br />・同時に何枚も見ることができる<br />・現在位置がわかりやすい<br />・メモを書き込むことができる（文字だけでなく矢印なども）<br />などがあります。<br /><br />紙は検索ができないので、PCを併用する必要があります。そのため、行番号の印刷は必須です。<br /><br /><br />Macで印刷する場合、enscriptとpstopdfを使っています。日本語未対応らしいのですが、幸いにも必要になったことはありません。<br /><br /><span  style="color: rgb(77, 77, 77); font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 14px; background-color: rgb(250, 249, 242);">enscript -j -r -2 --highlight=python --line-numbers -o - emscripten.py |pstopdf -i -o&nbsp;</span><span  style="color: rgb(77, 77, 77); font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 14px; background-color: rgb(250, 249, 242);">emscripten</span><span  style="color: rgb(77, 77, 77); font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 14px; background-color: rgb(250, 249, 242);">.py.pdf</span><span  style="color: rgb(77, 77, 77); font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 14px; background-color: rgb(250, 249, 242);">&nbsp;</span><br /><br />これで、A4用紙１ページに120行ぐらい印刷できます。<br /><br />10枚程度なら家庭用プリンタで、それ以上ならコンビニかKinko'sへゴー。紙代ケチって両面印刷にするのは、閲覧性が下がるのでおすすめできません。代わりに字を小さくしましょう。<br /><br />必要なことが大体把握できたら本棚にでもしまっておいて、適当な時間がたったら捨てます。<br /><br />

