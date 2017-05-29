---
layout: post
title: "libxml2の使い方メモ(2)"
date: 2009-02-20 14:56
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172434.html
---

・xmlGetPropしたらxmlFreeしなければならない。<br>
内部的にコピーして返してくれるので、解放する必要がある。<br>
xmlNodeGetBase, xmlNodeGetContent xmlNodeGetLangなども同様。<br>
<br>
めんどい…<br>


