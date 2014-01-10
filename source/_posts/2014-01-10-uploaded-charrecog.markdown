---
layout: post
title: "CharRecog を GitHub にアップしました"
date: 2014-01-10 22:20:53 +0900
comments: true
categories: [ニューラルネット,文字認識,プログラム,C++,OpenCV]
---
ニューラルネットによる文字認識プログラム CharRecog を
[GitHub にアップしました](https://github.com/kensak/CharRecog)。

特徴としては、
+ C++ で作成。OpenCV の ANN_MLP クラスを参考に、コアの部分はゼロから書きなおしました。
+ 並列処理 : インテル TBB、OpenMP, C++ AMP などを使い、マルチコア CPU や GPU による並列処理をおこなっています。
+ 行列計算による高速化 : すべてのサンプル画像を処理する際、できるだけループを回さず一回の行列計算により処理をおこないます。
+ さまざまな手法への対応 : autoencoding, convolutional layer, max pooling, maxout, dropout の各手法や
  入力画像のランダムな affine 変換などに対応しています。
+ ビルド済み実行ファイルでは float で計算をおこないますが、ソースを一ヶ所変更してリビルドすれば double にも対応します。
+ 学習された重みを画像として出力できます。

すぐに使えるよう 64 ビット版のバイナリもアップしています。
MNIST 画像セットを使って autoencoding や dropout などの実験が簡単におこなえます。

今後も気になる手法があればインプリメントしていこうと考えています。
また、画像認識や言語処理のための高速なNNエンジンとしても使えるようにしていきたいです。




