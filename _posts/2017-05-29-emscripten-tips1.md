---
layout: post
title: "Emscripten tips(1)：生成されるファイルの構造"
author: fchiba
---

Emscriptenのコマンドを実行して、生成されるファイルの構造について解説します。一度くらい自分で実行したことがないと、読んでもわからないかも。

これを読むと、意図しない動作が発生している場合のデバッグに役立つかもしれません。


サンプル用のプログラムはこちら。
```
#include <stdio.h>

int main(int argc, char* argv[])
{
    puts("Hello world!");
}
```

これを
```
emcc -o test.html test.c
```
というコマンドでコンパイルすると、test.htmlとtest.jsという２つのファイルが生成されます。
test.htmlをブラウザで開くと、下の方にコンソール出力用のtextareaがあり、そこにHello world!と表示されます。
生成されたファイルは[gist](https://gist.github.com/fchiba/10b2d6553133c6f6984f178a1a8fd809)に置いて起きます。

ではこれらのファイルの構造を見ていきます。ファイルの役割ですが、test.jsが処理の本体で、test.htmlは実行結果を表示するためのビューです。

test.htmlは一見巨大に見えますが、記述内容のほとんどを占めているのは左上に表示されているロゴ用のSVGなので恐れる必要はありません。重要なのは、[最後のscript要素２つ]( https://gist.github.com/fchiba/10b2d6553133c6f6984f178a1a8fd809#file-test-html-L1219-L1298)です。一番最後のscriptタグはtest.jsの読み込み用です。
一つ手前のscript要素は、test.jsの動作のカスタマイズをするためのものです。例えば、stdoutへの出力はデフォルトではconsole.logへ出力されるのですが、ここではtextareaへも出力されるようにModule.printをカスタマイズしています。詳しくは[ドキュメント]( http://kripken.github.io/emscripten-site/docs/api_reference/module.html?highlight=module)を参照。

test.jsは大きく３つのパートに分けられます。[真ん中](https://gist.github.com/fchiba/10b2d6553133c6f6984f178a1a8fd809#file-test-js-L1831-L5978)にasm.jsモジュールがあり、その前と後に通常のJavaScriptがあります。
emscriptenはご存知の通り、asm.jsをベースとした技術です。asm.jsは言ってみれば巨大なヒープ（ArrayBuffer）の上で計算だけを高速に行うためのものなので、I/Oなどの計算以外の部分はJSの関数を作ってそれをasm.jsモジュールに渡してやる必要があります。asm.jsモジュールより手前の部分でそれらの準備を行い、asm.jsより後の部分でasm.jsモジュールからexportされた関数を呼び出して実行します。手前の部分はpreamble、後の部分をpostambleと呼び、それぞれemscriptenのsrc/preamble.js, src/postablem.jsの中で定義されています。

## preamble
ざっくり分けると、以下のような関数群に分類されます。

- 標準入出力：printやreadなどがプラットフォームごとに定義され、一つのソースファイルが複数のプラットフォームで同時に動くようになっています。
- メモリ操作：スタックやヒープの操作用関数が準備されています。
- 関数テーブルとその操作：関数ポインタは、ポインタ値と関数オブジェクトの対応付けを行う配列によって実現されています。テーブル自体を操作するための関数や、JavaScriptからC関数を呼び出すためのインターフェースなどが用意されています。・文字列操作：ヒープ上の文字列とJavaScriptの文字列の相互変換をするための関数などが用意されています。
- 初期化/後始末：main関数の呼び出し前や終了後に実行すべき処理を登録するためのキューとその操作用の関数群が用意されています。グローバル変数のイニシャライザや、ダイナミックリンク、ファイルシステムの初期化などもこの仕組みを利用しています。
- ヒープ初期化：今回は特に何もオプションを指定しなかったので、[テキスト](https://gist.github.com/fchiba/10b2d6553133c6f6984f178a1a8fd809#file-test-js-L1630)で書かれた配列で初期化されています。"--memory-init-file 1"というオプションをつけると、バイナリファイルとして切り出すことができます。
- ブラウザ機能の呼び出し：ファイルシステム、OpenGL、サウンドなどブラウザとやりとりするような機能については、emscriptenのsrc/libarary_*.jsに定義されており、必要に応じてその中身が展開されます。今回はputsだけしか使っていないので、一部のシステムコールが展開されています。
	
実際は、いろいろな関数が乱雑に並んでいるだけのように見えて結構カオスです。正直もう少しなんとか整理できないものかとは思いますが、いまさら難しいかも…

## asm.jsモジュール
何をどのように書くかはspecificationで順番が決まっています。[このへん](http://defghi1977.html.xdomain.jp/tech/asmjs/asmjs.htm)見ましょう。

## postamble
ここは比較的シンプルです。

- Moduleへのexport：asm.jsモジュールで定義された関数をModuleに代入して、JavaScriptから利用できるようになっています。
- main関数の呼び出し：最後にasm.jsモジュール内のmain関数が呼ばれます。
