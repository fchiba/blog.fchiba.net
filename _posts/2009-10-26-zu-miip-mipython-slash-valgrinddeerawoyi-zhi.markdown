---
layout: post
title: "組み込みPython/valgrindでエラーを抑制"
date: 2009-10-26 12:34
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172458.html
---

C++&SWIGでPythonを拡張したとき、拡張した部分にメモリリークがないかをvalgrindでチェックしたい。<br>
普通にpythonに対して、valgrindを使うと大量にエラーが出てくるが、これを抑制する方法がある。<br>
<br>
http://svn.python.org/projects/python/trunk/Misc/valgrind-python.supp<br>
http://svn.python.org/projects/python/trunk/Misc/README.valgrind<br>
<br>
valgrind-python.supp の冒頭の翻訳：<br>
<br>
<blockquote>#<br>
# このファイルは、valgrindを使うときに使うべき valgrind 用（エラー）抑制ファイルです。<br>
#<br>
# 使い方の例：<br>
#<br>
#	cd python/dist/src<br>
#	valgrind --tool=memcheck --suppressions=Misc/valgrind-python.supp \<br>
#		./python -E -tt ./Lib/test/regrtest.py -u bsddb,network<br>
#<br>
# Py_ADDRESS_IN_RANGE に関するエラー抑制を行うためには、<br>
# Objects/obmalloc.cを編集して、Py_USING_MEMORY_DEBUGGERをアンコメントする必要があります。<br>
#<br>
# もしPythonのリコンパイルがいやなら、抑制ファイル中のPyObject_Free とPyObject_Reallocに関する部分をアンコメントする必要があります。<br>
#<br>
# より詳しい情報は、Misc/README.valgrind を見てください。<br>
</blockquote><br>
<br>
デフォルトで抑制するには、~/.valgrindrc に書いておく。<br>


