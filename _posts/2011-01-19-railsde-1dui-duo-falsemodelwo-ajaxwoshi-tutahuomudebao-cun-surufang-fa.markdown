---
layout: post
title: "Railsで、１対多のmodelを、Ajaxを使ったフォームで保存する方法"
date: 2011-01-19 12:29
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172475.html
---

１対多の親子関係のモデルをWebから入力する際に、１つのフォーム（nested object formsと呼ぶらしい）で済ませたい場面がある。例えば請求書の入力画面みたいなやつ。<br>
そういう場合はaccepts_nested_attributes_forとfields_forを使えばできるよ～、というサンプルはWeb上にたくさん見つかる。<br>
<br>
しかし、ほとんどは子の入力欄の数が固定のものばかり（追加したい分だけ事前にコントローラーで子のモデルをbuildしておく必要がある）で、JavaScriptつかって入力欄を増減させたりするサンプルはなかなか見つからない。あるにはあるけど、トリッキーなコードだったり冗長だったりする。FormBuilderの取り回しやaccepts_nested_attributes_forの受け付けるパラメータの形式などを考慮すると、シンプルに書くのはけっこう難しい。<br>
<br>
で、ようやく見つけたのがこれ。<br>
<a href="http://railscasts.com/episodes/197-nested-model-form-part-1" target="_blank" title="http://railscasts.com/episodes/197-nested-model-form-part-1">http://railscasts.com/episodes/197-nested-model-form-part-1</a><br>
<a href="http://railscasts.com/episodes/197-nested-model-form-part-2" target="_blank" title="http://railscasts.com/episodes/197-nested-model-form-part-2">http://railscasts.com/episodes/197-nested-model-form-part-2</a><br>
ただし、Rails 3 の場合、ApplicationHelper#link_to_add_fieldsの最後のlink_to_functionの第２引数のhはとる必要がある（3 からデフォルトでescapeされるので２重にかかってしまう）。<br>


