---
layout: post
title: "libxml2の使い方メモ"
date: 2009-02-18 21:38
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172432.html
---

libxml2をC言語で使うときのメモ。<br>
<br>
・parseの仕方は<a href="http://xmlsoft.org/examples/index.html" target="_blank" title="examples">examples</a>を参照（たくさん例がある）<br>
<br>
・逆にツリーをたどったりするサンプルはほとんどないので、google code searchを頼る。<br>
<br>
・「ある名前を持った子要素を一つ取り出す」という関数はないため、<br>
<blockquote><p>xmlNode *get_child (xmlNode *node, const char *tag)<br>
{<br>
  xmlNode *child;<br>
<br>
  for (child = node->children; child != NULL; child = child->next)<br>
    if (!strcmp ((char *)child->name, tag))<br>
      return child;<br>
  return NULL;<br>
}<br>
</p></blockquote><br>
のようなユーティリティ関数を多くのプロジェクトで書いている。<br>
<br>
・XPathで相対ロケーションパスを使う場合、コンテキストのノードにカレントノードを指定する<br>
<blockquote><p>xpathCtx = xmlXPathNewContext(doc);<br>
xpathCtx->node = currentNode;<br>
xpathObj = xmlXPathEvalExpression(...xpath..., xpathCtx);<br>
</p></blockquote><br>
<br>
・RelaxNGによるバリデーションについても情報は少ない。<br>
<br>
<blockquote><p><br>
    // RelaxNGパーサーコンテキストの生成<br>
    xmlRelaxNGParserCtxtPtr ctxt = xmlRelaxNGNewMemParserCtxt(rng_string, strlen(rng_string));<br>
    // ファイルからなら、docを作ってxmlRelaxNGNewDocParserCtxtを使う。<br>
<br>
    // RelaxNGパーサーの生成<br>
    xmlRelaxNGPtr relaxngschemas = xmlRelaxNGParse(ctxt);<br>
    if(relaxngschemas == NULL) {<br>
        // エラー処理<br>
    }<br>
    xmlRelaxNGFreeParserCtxt(ctxt);<br>
    <br>
    // 実際のバリデーション<br>
    xmlRelaxNGValidCtxtPtr vctxt = xmlRelaxNGNewValidCtxt(relaxngschemas);<br>
    int ret = xmlRelaxNGValidateDoc(vctxt, doc);<br>
    if (ret != 0) {<br>
        // エラー処理<br>
    }<br>
    xmlRelaxNGFreeValidCtxt(vctxt);<br>
</p></blockquote><br>
<br>
・RelaxNGのexceptは未実装。<br>
<br>
・libxml2に付属のxmllint.cは参考になる。<br>


