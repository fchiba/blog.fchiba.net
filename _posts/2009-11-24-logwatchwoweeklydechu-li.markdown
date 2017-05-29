---
layout: post
title: "logwatchをweeklyで出力"
date: 2009-11-24 12:59
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172462.html
---

サーバーを何台も管理しているとlogwatchのメールが大量になり、ほとんど読まなくなってしまう。<br>
止める、というのも一つの手だが何も見ないのも不安。<br>
<br>
ということで、weeklyにして読む手間を軽減させてみようと試みた。<br>
<br>
まずは集計期間がデフォルトだと実行日の前日なので、それを直前1週間にする。<br>
<br>
<a href="http://list.logwatch.org/pipermail/logwatch/2007-October/001583.html" target="_blank" title="http://list.logwatch.org/pipermail/logwatch/2007-October/001583.html">http://list.logwatch.org/pipermail/logwatch/2007-October/001583.html</a><br>
によると、<br>
logwatch --service postfix --range "between -7 days and -1 days"<br>
のように、rangeを書けばよいらしい。<br>
ところが、<br>
<blockquote><p>This system does not have Date::Manip module loaded, and therefore<br>
the only valid --range parameters are 'yesterday', 'today', or 'all'.<br>
</p></blockquote><br>
ということで、Date::Manip を入れないとだめらしい。ところが、指示通り<br>
<blockquote><p>cpan -i 'Date::Manip'<br>
</p></blockquote><br>
としても、<br>
<blockquote><p>Can't locate feature.pm in @INC<br>
略<br>
at /root/.cpan/build/Date-Manip-6.00/blib/lib/Date/Manip/Base.pm line 22<br>
</p></blockquote><br>
というエラーが出てインストールできず、該当箇所を見ると、<br>
<blockquote><p>use feature "switch";<br>
</p></blockquote><br>
とあり、これが今使っているperlのバージョンでは使えないらしい。(CentOS 5.3 でver 5.8.8)<br>
<br>
旧バージョンの Date::Manipならどうかと<br>
<a href="http://vlog.blog32.fc2.com/blog-entry-111.html" target="_blank" title="http://vlog.blog32.fc2.com/blog-entry-111.html">http://vlog.blog32.fc2.com/blog-entry-111.html</a><br>
を参考にして試したところ見事成功。<br>
<br>
<br>
あとは、設定を /etc/logwatch/conf/logwatch.conf に書き、/etc/cron.daily/ にある 0logwatch を /etc/cron.weekly に移動して完了。<br>
<br>
コマンド：<br>
<blockquote><p>cpan S/SB/SBECK/Date-Manip-5.54.tar.gz<br>
echo Range = between -7 days and -1 days >> /etc/logwatch/conf/logwatch.conf <br>
mv /etc/cron.daily/0logwatch /etc/cron.weekly/<br>
</p></blockquote><br>


