---
layout: post
title: "opensslのCAシェルとopenssl.cnf"
date: 2009-07-03 14:29
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172448.html
---

opensslで内部用のCAを作るときに、CAシェル(/etc/pki/tls/misc/CA)とopenssl.cnfをカスタマイズする必要があるが、<br>
そのときのメモ。(CentOS 5で実行)<br>
<br>
<hr size="1" /><br>
・openssl.cnf<br>
設定の内容は、<br>
http://www.technoids.org/openssl.cnf.html<br>
を参照。<br>
<br>
・ca, req, x509 コマンドで参照される<br>
・caコマンドでは[ca]セクションが、reqコマンドでは[req]セクションが読み込まれる<br>
・内部で他のセクションを参照できる<br>
・-name, -extensions といったオプションで、参照設定を上書きできる<br>
・デフォルトでは、caコマンドはusr_certセクションを、reqコマンドはv3_caセクションを参照している。<br>
<br>
<hr size="1" /><br>
・CA<br>
環境変数のCATOPはnewcaでディレクトリを作るときのみ使われる。あとは、openssl.cnfの値が使われる。<br>
CAKEY, CAREQ, CACERTは相対パスでないといけない。<br>
<br>
設定ファイルをカスタマイズした場合、<br>
SSLEAY_CONFIG="-config ファイル名"<br>
とする。<br>
<br>
newcaは、<br>
１．秘密鍵とCSRの生成<br>
２．１．で生成したCSRに、同じく１．で生成した秘密鍵で自己署名<br>
という手順を踏む。<br>
２．の署名はcaコマンドを使うため、そのままではopenssl.cnfのusr_certセクションを参照してしまい、CA用の証明書が生成されないにならない（basicConstraints=CA:FALSEになってしまう）。<br>
多くのサイトでは、usr_certセクションを直接書き換えて対応しているが、v3_caセクションを参照するようにした方がエレガントではないかと思う。<br>
ということで、<br>
<pre>            $CA -out ${CATOP}/$CACERT $CADAYS -batch -extensions v3_ca \<br>
                           -keyfile ${CATOP}/private/$CAKEY -selfsign \<br>
                           -infiles ${CATOP}/$CAREQ </pre><br>
とした。<br>


