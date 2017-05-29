---
layout: post
title: "Apache Moduleの作り方(2)"
date: 2009-02-27 00:56
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172438.html
---

・その６<br>
Segmentation Faultなどが発生してもログには1行しか吐かれないし、デーモンをgdbでデバッグするのは大変。（面倒であきらめた）<br>
せめて簡単にバックトレースを出したい、ということで<br>
mod_backtrace<br>
を使った。<br>
ソースは<a href="http://people.apache.org/~trawick/mod_backtrace.c" target="_blank" title="ここ">ここ</a>から取ってくる。<br>
<br>
これをコンパイルするためには、<br>
--enable-exception-hook<br>
というオプションを付けて、apacheをconfigureしないといけないらしい。<br>
apacheはパッケージからインストールしているので、src RPMをいじって対処することにする。<br>
<br>
１．リポジトリ（<a href="http://ftp.iij.ad.jp/pub/linux/centos/5.2/updates/SRPMS/" target="_blank" title="IIJ">IIJ</a>とか）からsrc.rpmをとってきて、<br>
rpm -ivh httpd-2.2.3-11.el5_2.centos.4.src.rpm<br>
などとしてソースをインストール。（事前にmkdir /usr/src/redhatしておく）<br>
２．/usr/src/redhat/SPECS/httpd.specにあるspecファイルから、configureをしている箇所を探し<br>
--enable-exception-hook<br>
を加える。<br>
３．rpmbuild -bb --clean /usr/src/redhat/SPECS/httpd.spec<br>
でhttpdとhttpd-develのrpmができる。<br>
（バージョンを変えていないので紛らわしいが自分だけしか使わないので気にしなーい）<br>
rpm -Uvh で上書きインストール。<br>
<br>
４．モジュールは<br>
apxs -ic mod_backtrace.c<br>
でインストールする。<br>
<br>
５．httpd.confに<br>
LoadModule backtrace_module modules/mod_backtrace.so<br>
EnableExceptionHook On<br>
を加える。<br>
<br>
参考にしたサイト<br>
カスタムRPMの作成第1回：カスタムRPMの基礎とspecファイル<br>
http://www.stackasterisk.jp/tech/systemManagement/rpm01_01.jsp<br>
カスタムRPMの作成第2回：rpmbuildとtarballからのカスタムRPM作成<br>
http://www.stackasterisk.jp/tech/systemManagement/rpm02_01.jsp<br>


