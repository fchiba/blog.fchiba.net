---
layout: post
title: "iPhoneオライリー本アプリをkindlegenするときにエラー"
date: 2010-07-17 01:55
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172473.html
---

<a href="http://d.hatena.ne.jp/shunsuk/20100715/1279199789" target="_blank" title="[ガジェット]たった600円でオライリー本をiPadやKindleで読む。すてき。- このブログは証明できない。">[ガジェット]たった600円でオライリー本をiPadやKindleで読む。すてき。- このブログは証明できない。</a><br>
<br>
を真似してkindle用ファイルを作ってみたら、少しはまったのでメモ。<br>
<br>
購入したのは Programming PHP（エクステンションの作り方が知りたかった）。<br>
<br>
まず、<br>
> Payloadというフォルダの中にappファイルがあります。<br>
は、Windowsだと "(アプリ名).app" フォルダだった。<br>
<br>
また、kindelgen を実行すると、<br>
Error(prcgen): TOC section scope is not included in the parent chapter:BCMath Arbitrary Precision Mathematics　（"BCMath～"というのは書籍内に含まれる章の名前）<br>
というエラーが発生した。<br>
<br>
エラーメッセージでググッたところ<br>
<a href="http://www.mobipocket.com/forum/viewtopic.php?p=49267&amp;sid=494a2df12b48a1e8fdca8ae0e7bfb752" target="_blank" title="http://www.mobipocket.com/forum/viewtopic.php?p=49267&amp;sid=494a2df12b48a1e8fdca8ae0e7bfb752">http://www.mobipocket.com/forum/viewtopic.php?p=49267&sid=494a2df12b48a1e8fdca8ae0e7bfb752</a><br>
がヒット。どうも、「toc.ncx」と「content.opfのspine要素」の整合性が取れていないためらしい。<br>
<br>
toc.ncxの該当部分は<br>
<pre>        &lt;navPoint id="id2424422" playOrder="1004"&gt;<br>
          &lt;navLabel&gt;<br>
            &lt;text&gt;B.1. Optional Extensions Listing&lt;/text&gt;<br>
          &lt;/navLabel&gt;<br>
          &lt;content src="apb.html#progphp2-APP-B-SECT-1"/&gt;<br>
          &lt;navPoint id="id2424885" playOrder="1005"&gt;<br>
            &lt;navLabel&gt;<br>
              &lt;text&gt;BCMath Arbitrary Precision Mathematics&lt;/text&gt;<br>
            &lt;/navLabel&gt;<br>
            &lt;content src="re514.html"/&gt;<br>
          &lt;/navPoint&gt;</pre><br>
となっているが、id2424422とid2424885の両方ともspineに含まれていない。<br>
（id2424422はアンカー指定となっているためかmanifest要素にさえ含まれていない。apb.html自体はさらに親のid2424369として含まれている）<br>
<br>
試行錯誤の結果わかったのは、navPointに含まれるidは親も子もspineの中に記述されていないと駄目らしいということ。<br>
<br>
・content.opf の spine にid2424885(子)を追加。<br>
・toc.ncxのid2424422(親)のIDをid2424369(apb.htmlのID)に書き換え。<br>
<br>
でコンパイルを通りmobiファイルが生成された。<br>
kindle for PCで動作を確認したところ、本文は正しく表示された。ただ、目次のほうはkindle for PC に機能自体がないのでうまく動いているかどうか不明。<br>
<br>
余談：epubファイルを作成するとき、本来はmimetypeを一番初めに無圧縮で格納しないといけないのだが、そこは適当でもkindlegenは大丈夫だった。<br>


