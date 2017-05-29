---
layout: post
title: "EditorConfigに関する雑多なメモ"
date: 2017-05-09 13:43
comments: 
categories: 
author: fumiya_chiba
permalink: archives/1065898560.html
---

<span  style="font-size: large;">仕様の曖昧さについて</span><br /><br />仕様は <a  target="_blank" href="http://editorconfig.org/#file-format-details">http://editorconfig.org/#file-format-details</a> だが、いくつか書かれていない暗黙のルールがある。<br />・セクションに "/" が含まれていない場合、先頭に "**/"がついているとみなす。<br />　（これがないと[*.js]と書いた場合にサブディレクトリ内のファイルにマッチしない）<br />・同じファイル内で複数のセクションに該当する場合はあとから設定された値で上書きされる<br /><br />他にもある気がするが、何が正しいかはコアライブラリの実装を見るしかない。コアライブラリは各言語ごとに用意されているが、言語ごとに微妙に動作が違ったりする（固い仕様がないので当然といえば当然）。どうもPyhton版かC版を正として移植されているような雰囲気。上記のルールは<a  target="_blank" href="https://github.com/editorconfig/editorconfig-core-py">Python版</a>の動作をもとにしている。<br /><br /><span  style="font-size: large;"><br />独断と偏見のコマンドラインツール選び</span><br /><br />普通に使うぶんには、エディタのプラグインを使えばいいのだが、既存プロジェクトの導入時に一括でチェック・修正したりとかCIで使う場合には、コマンドラインツールを使いたい。他にも、既存のファイルからルールを自動生成できると便利。<br /><br />コマンドラインツールですぐに見つかるのは次の３つ。<br /><a  target="_blank" href="https://github.com/jedmao/eclint">https://github.com/jedmao/eclint</a><br /><a  target="_blank" href="https://github.com/treyhunner/editorconfig-tools">https://github.com/treyhunner/editorconfig-tools</a><br /><a  target="_blank" href="https://github.com/slang800/editorconfig-tools">https://github.com/slang800/editorconfig-tools</a><br /><br />eclintはインデント判定でブロックコメントやヒアドキュメントを認識してくれない（＆作者は対応を拒否している）ので使いづらい。言語ごとに手を入れたらキリがないのは分かるが、それでは使い物にならないのでパス。<br />treyhunner/editorconfig-toolsは、生成機能がない。READMEに"--generate"とかあるけど、あれはただの構想で、欠片すら実装されてない。<br />slang800/editorconfig-toolsはそこそこ使える。インデントは<a  target="_blank" href="https://github.com/sindresorhus/detect-indent">detect-indent</a>で判定。100%正確ではなさそうだが、実用的な範囲内では使えそう。<br /><br />ということで、slang800/editorconfig-toolsを使う予定。<br /><br /><span  style="font-size: large;">設定の削除<br /></span><br />バッチで一括処理する際に、プロジェクト外から来たファイルには手を加えたくない事が多い。<br />対象ファイルの列挙時に除外する方法と、.editorconfigで除外する方法があるが、後者で行う場合は設定値にunsetを使うとよい。（昔は、"null"などのでたらめな値をいれると、パースできずに処理不能になることを利用して無効化していた模様。今はオフィシャルにunsetが使えることになっている）<br /><br />が、unsetはslang800/editorconfig-toolsで対応されていない…<br />→修正するPR送った。<br /><br />

