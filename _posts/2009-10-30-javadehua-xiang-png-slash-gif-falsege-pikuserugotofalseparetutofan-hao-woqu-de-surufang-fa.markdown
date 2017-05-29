---
layout: post
title: "Javaで画像(PNG/GIF)の各ピクセルごとのパレット番号を取得する方法"
date: 2009-10-30 20:53
comments: 
categories: 
author: fumiya_chiba
permalink: archives/172459.html
---

Javaで画像(PNG/GIF)の各ピクセルごとのパレット番号を取得する方法<br>
<br>
<pre>BufferedImage image = ImageIO.read(new File("1.gif"));<br>
// 「色」の情報を表すクラス<br>
//パレット形式の場合は ColorModelが IndexColorModelのインスタンスになる<br>
ColorModel model = image.getColorModel();<br>
if(model instanceof IndexColorModel) {<br>
	IndexColorModel icm = (IndexColorModel)model;<br>
	// パレットの大きさ。未使用の部分があっても、常に256になってしまう…<br>
	System.out.println(icm.getMapSize());<br>
	// パレットのデータ。RGBAがintにパックされる。<br>
	int[] rgb = new int[icm.getMapSize()];<br>
	icm.getRGBs(rgb);<br>
	for(int i=0;i&lt;rgb.length;i++) {<br>
		Color c = new Color(rgb[i]);<br>
		System.out.printf("%d,%d,%d,%d\n",c.getRed(),c.getGreen(),c.getBlue(),c.getAlpha());<br>
	}<br>
	int minx = image.getMinX();<br>
	int miny = image.getMinY();<br>
	int width = image.getWidth();<br>
	int height = image.getHeight();<br>
	// 「点」の情報を表すクラス<br>
	Raster raster = image.getRaster();<br>
	// バンド幅。RGBなら3、RGBAなら４になる。<br>
	// IndexColorModelの場合は常に1になる（はず）。<br>
	int band = raster.getNumBands();<br>
	if(band != 1) {<br>
		throw new RuntimeException("unexpected bands");<br>
	}<br>
	// 実際のピクセルごとのデータ。パレットの何番に該当するかが入っている<br>
	// 要素数はwidth*height<br>
	// （フルカラーの場合はRGB(A)の数値が入っていて、要素数はbands*width*height）<br>
	int[] buf = raster.getPixels(minx, miny, width, height, (int[])null);<br>
	for(int y=0;y&lt;height;y++) {<br>
		for(int x=0;x&lt;width;x++){<br>
			System.out.printf("%02d",buf[x+y*width]);<br>
			System.out.print(",");<br>
		}<br>
		System.out.println("");<br>
	}<br>
}</pre><br>
<br>
<br>
最初のパレットかフルカラーかどうかの判定はif(image.getType() == BufferedImage.TYPE_BYTE_INDEXED) (=13)でも可能。<br>
（ちなみに、JPEGの場合はBufferedImage.TYPE_3BYTE_BGR(=5)、フルカラーPNGの場合、BufferedImage.TYPE_CUSTOM (=0) だった。アルファがあったりするとまた変わるらしい）<br>


