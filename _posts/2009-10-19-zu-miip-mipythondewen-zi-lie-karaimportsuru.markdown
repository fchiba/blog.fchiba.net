---
layout: post
title: "組み込みPythonで文字列からimportする"
date: 2009-10-19 19:42
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172455.html
---

PyImport_ExecCodeModuleでモジュールを実行した後、importしないと使えない。<br>
<br>
<pre>#include &lt;Python.h><br>
<br>
int main(void) {<br>
	Py_Initialize();<br>
	<br>
	FILE *fp;<br>
	<br>
	const char* module_code =<br>
		"class Hello:\n"<br>
		"	def __init__(self, message):\n"<br>
		"		self.message = message\n"<br>
		"	def say(self):\n"<br>
		"		print 'hello %s' % self.message\n"<br>
	;<br>
	<br>
	// エラーハンドリング省略<br>
	PyObject* compiled_code = Py_CompileString(module_code, "hello.py", Py_file_input);<br>
	PyObject* module = PyImport_ExecCodeModule("hello", compiled_code);<br>
	Py_XDECREF(compiled_code);<br>
	<br>
//	↓これでは動かない<br>
//	PyImport_ImportModule("hello");<br>
	<br>
	const char* main_code =<br>
		"import hello\n"              // ←これが必要<br>
		"h = hello.Hello('world')\n"<br>
		"h.say()"<br>
	;<br>
	<br>
	PyRun_SimpleString(main_code);<br>
	<br>
	Py_Finalize();<br>
	<br>
	return 0;<br>
}<br>
</pre><br>


