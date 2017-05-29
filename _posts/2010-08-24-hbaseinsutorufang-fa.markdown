---
layout: post
title: "HBaseインストール方法"
date: 2010-08-24 13:53
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172474.html
---

最近、仕事でHadoopを使うことになりました。<br>
Hadoopに関しては、Hadoop本<br /><table style="width:75%;border:0;" border="0"><tr><td style="border:none;" valign="top" align="center"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/487311439X/fchiba-22/ref=nosim/" target="_blank"><img src="http://ecx.images-amazon.com/images/I/51ecKX5HnQL._SL75_.jpg" alt="Hadoop" border="0"></a></td><td style="padding:0 0.4em;border:0;" valign="top"><a href="http://blog.fc2.com/goods/487311439X/fchiba-22" target="_blank">Hadoop</a><br />(2010/01/25)<br />Tom White<br /><br /><a href="http://www.amazon.co.jp/exec/obidos/ASIN/487311439X/fchiba-22/ref=nosim/" target="_blank">商品詳細を見る</a></td></tr></table><br>
を読めばだいたいのことはわかるでしょう。<br>
（訳がこなれていない部分があるので、英語版も念のため買っておくと便利です。iPhone版が安くてお薦め）<br>
<br>
Hadoopで集計した結果を保存／参照するのに、HBaseを使おうと思っています。<br>
分散環境でのパラメータが分かりにくかったので、<a href="http://hbase.apache.org/docs/current/api/overview-summary.html#getting_started" target="_blank" title="Getting Started">Getting Started</a>を超訳してみたいと思います。<br>
<br>
<hr size="1" /><br>
<h1>始めるには</h1><br>
まずはファイルをダウンロードすべし。<br>
Hadoop本体と同様、スタンドアローン、擬似並列、完全並列の３つのモードがある。<br>
まずは、Requirementsを読むこと。<br>
<span style="color:#FF0000">HBASE_HOME</span>と<span style="color:#FF0000">JAVA_HOME</span>を設定すべし。<br>
<br>
<h2>スタンドアローン</h2><br>
デフォルト値はスタンドアローン用になっているので、そのままでOK。<br>
<br>
<h2>擬似並列、完全並列</h2><br>
Hadoop Distributed File System (DFS)を使うので、そちらのセットアップを先に済ませること。<br>
<br>
<h3>擬似並列</h3><br>
HBaseはhbase-default.xmlに全ての設定値がかかれており、必要なものだけhbase-site.xmlで上書きする。<br>
擬似並列で最低限必要なものは、<span style="color:#FF0000">hbase.rootdir</span>で、hdfs://localhost:9000/hbase などとする。<br>
<br>
/hbaseディレクトリはHBaseが初期化するので、自分で作らなくてもよい。<br>
<br>
<h3>完全並列</h3><br>
まず、hbase-site.xmlの<span style="color:#FF0000">hbase.cluster.distributed</span>をtrueにする。<br>
次に、<span style="color:#FF0000">${HBASE_HOME}/conf/regionservers</span>にRegion Server（実際にデータを保存するサーバー）を１行１サーバーで記入。${HADOOP_HOME}/conf/slavesと同じようなもの。<br>
<br>
続いてZooKeeperの設定（ZooKeeperはHBaseの各ノードが協調して動くために使う）。HBaseの各ノードからもクライアントからもアクセス可能になっている必要がある。HBase専用で動かす方法と、事前に用意したZooKeeperクラスタを利用する方法がある。前者にするには、${HBASE_HOME}/conf/hbase-env.shの<span style="color:#FF0000">HBASE_MANAGES_ZK</span>をtrueにする。後者にするにはfalseにする。<br>
<br>
ZooKeeperの設定は、ZooKeeperのzoo.cfgで行う方法と、HBaseのhbase-site.xml内にhbase.zookeeper.property.OPTIONを書く方法がある。（HBaseを初めて使うなら後者がお薦め。何が指定できるかはhbase-default.xmlを参照）<br>
<br>
最小限必要なのは、<span style="color:#FF0000">hbase.zookeeper.quorum</span>。quorumはZooKeeperを構成するサーバーのこと。quorumは3台or5台or7台とし、各サーバーにRAM1GBと（可能であれば）専用のディスクを割り当てることを推奨。カンマ区切りでマシン名を列挙する。<br>
<br>
また、ZooKeeperのデータの保存ディレクトリのデフォルト値が/tmp配下になっているので、<span style="color:#FF0000">hbase.zookeeper.property.dataDir</span>で変えること。<br>
<br>
HBASE_MANAGES_ZKがtrueの場合、ZooKeeperの起動／停止はHBaseの起動／停止中に自動的に行われる。自分でやる場合は、<br>
${HBASE_HOME}/bin/hbase-daemons.sh {start,stop} zookeeper<br>
で可能。<br>
<hr size="1" /><br>
<br>
はまった点。<br>
その１：hbase.zookeeper.property.dataDirのディレクトリを作ってなかった。<br>
その２：AmazonEC2で試していたが、hbase.zookeeper.quorumにはpublicなほうのホスト名を入れていたが、ZooKeeperがhostnameで取ってきたホスト名（privateなほう）を内部的に使うらしく動かなかった。<br>
その３：ZooKeeperが動かないと RegionServerへの接続がうまくいかず、Masterから停止ができず面倒（手でkillした）<br>


