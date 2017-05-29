---
layout: post
title: "組み込みPython/パッケージ名つきでimportする"
date: 2009-10-20 20:26
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172456.html
---

SWIGで単純にC/C++のラッパーを作ると、1階層のモジュールしかできない。<br>
<br>
これをパッケージ名つきでimportするには、xxxx_wrap.cxx を以下のように修正する。<br>
<br>
例）helloモジュールをaaa.bbb.hello としたい場合<br>
<br>
hello_wrap.cxx<pre>#define SWIG_name    "_hello"</pre>→<pre>#define SWIG_name    "aaa.bbb._hello"</pre><br>
<pre>  m = Py_InitModule((char *) SWIG_name, SwigMethods);<br>
#endif<br>
  d = PyModule_GetDict(m);</pre>→<pre><br>
  PyObject* m_aaa = Py_InitModule("aaa", NULL);       // aaa を作る<br>
  PyObject* m_bbb = Py_InitModule("aaa.bbb", NULL);   // aaa.bbb を作る<br>
  m = Py_InitModule((char *) SWIG_name, SwigMethods); // aaa.bbb.hello を作る<br>
  <br>
#endif<br>
  PyObject* d_aaa = PyModule_GetDict(m_aaa);<br>
  PyObject* d_bbb = PyModule_GetDict(m_bbb);<br>
  d = PyModule_GetDict(m);<br>
  <br>
  PyDict_SetItemString(d_aaa, "bbb", m_bbb);            // aaa.bbb(変数) に bbb(モジュール) を登録<br>
  PyDict_SetItemString(d_bbb, "_hello", m);     // aaa.bbb.hello(変数) に _hello(モジュール) を登録<br>
</pre><br>
上位のモジュールがすでに存在する場合は、Py_InitModuleの代わりにPyImport_AddModuleを使う。<br>


