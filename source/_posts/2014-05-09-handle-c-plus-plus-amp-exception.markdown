---
layout: post
title: "C++ AMP の例外の処理"
date: 2014-05-16 13:45:05 +0900
comments: true
categories: [C++AMP]
---
C++ AMP を通して GPU 上で計算をしていると、TDR という仕組みによる例外が発生することがあります。

TDR とその対応については以下のページに詳しく解説されています。

[Handling TDRs in C++ AMP](http://blogs.msdn.com/b/nativeconcurrency/archive/2012/03/06/handling-tdrs-in-c-amp.aspx)

<!-- more -->

TDR による例外が発生したときに自動的に計算サイズを調整して
復帰する[コード](https://github.com/kensak/CharRecog/blob/master/src/NeuralNet.cpp)を紹介します。



elemMul_sub_GPU と elemMul という関数は2つの行列の要素ごとの掛け算を行ないます。
（どちらもパラメターとして2つの行列をとるものと3つの行列をとるものがあります。）

matMul_sub_GPU と matMul は行列の掛け算を行ないます。GPU が利用できない時は cv::gemm を呼び出します。

これらのコードは以下のライブラリに依存しています。

+ OpenCV 2.4.3  
  [ここ](http://sourceforge.net/projects/opencvlibrary/files/opencv-win/)からダウンロードできます。  
  
+ C++ AMP BLAS Library 1.0  
  [ここ](http://ampblas.codeplex.com/releases/view/92383)からダウンロードできます。  

rowBlockSize がブロックサイズです。行列を rowBlockSize 行ごとに切り分けて計算しています。
例外が発生するとブロックサイズを 1/2 にしてリトライします。ブロックサイズが 100 未満になるとあきらめます。
