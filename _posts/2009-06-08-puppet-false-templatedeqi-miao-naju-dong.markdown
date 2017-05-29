---
layout: post
title: "puppet の templateで奇妙な挙動"
date: 2009-06-08 19:33
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172446.html
---

puppetのtemplateで奇妙な挙動を発見。<br>
テンプレート中で<br>
<pre>&lt;% name ||= 'default_name' %><br>
</pre><br>
などと、変数のデフォルト値を設定しようとすると、manifestでどう書いてもdefault_nameが代入されてしまう。<br>
しかも、<br>
<pre>&lt;%=name%><br>
&lt;% name ||= 'default_name' -%><br>
&lt;%=name%><br>
</pre><pre>class xxx{<br>
  $name = 'true_name'<br>
  file{<br>
     content => template('test.erb')<br>
  }<br>
}<br>
</pre><br>
とすると、<br>
<pre>true_name<br>
default_name<br>
</pre><br>
なんて生成される。（erbを直接rubyから実行するとtrue_nameが2回生成される）<br>
puppet wikiを参考に<br>
<pre>&lt;% name = 'aaa' unless has_variable?('name') <br>
</pre><br>
とすると、なぜかnameがnilになってしまう。<br>
<br>
なぜ？？？<br>
とりあえず、<br>
<pre>&lt;%<br>
if has_variable?('name')<br>
    name2 = name<br>
else<br>
    name2 = 'default_name'<br>
end<br>
%></pre>とし、name2を使えば問題が回避できた。<br>


