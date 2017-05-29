---
layout: post
title: "NSURLConnectionを使って本来のサーバーとは違うサーバーに接続する"
date: 2013-09-22 14:11
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172486.html
---

テスト用にDNSに登録されているものとは違うサーバーにHTTP/HTTPSで接続したい場合の方法。ちなみにクライアントだと/etc/hosts使ってやることが多い。<br>
<br>
例えば https://www.example.com/ が本来は 192.0.2.1がDNSに登録されているとして、192.0.2.2に向き先を変えたい場合。<br>
<br>
リクエスト自体はIPアドレスで<br>
NSMutableURLRequest *request= [NSMutableURLRequest requestWithURL:@"https://192.0.2.2/path"]<br>
として作成し、<br>
[request setValue:@"www.example.com" forHTTPHeaderField:@"Host"];<br>
などとHostだけ上書きしてやればOK。<br>


