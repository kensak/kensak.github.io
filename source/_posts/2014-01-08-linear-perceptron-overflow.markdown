---
layout: post
title: "ニューラルネットでのオーバーフローについてのメモ"
date: 2014-01-08 22:03:03 +0900
comments: true
categories: [ニューラルネット,プログラミング,C++,OpenCV]
---
前回はニューラルネットの新たな（？）手法、maxout についてふれました。
これは tanh やシグモイドなどのアクティベーション関数を使うかわりに、
出力をいくつかづつにまとめてその max を計算し、レイヤーの出力とするというものでした。

この maxout レイヤーを複数重ねる場合、そのままでは重みが大きくなることによる浮動小数点の計算エラーが発生することがあります。
つまり、maxout では入力に重みを掛けるばかりで tanh のように値域を狭めるものがないので、出力が爆発してしまう。

これを避けるには、[1]にあるように重み行列のノルムに制限をかけ、制限を越えたら重みとバイアスに同じ係数を掛けてスケールダウンする必要があります。

<!-- more -->

これはOpenCVを使えばこんな風に書けます。
{% codeblock lang:cpp %}
double weightNorm = cv::norm(weight);
if (maxWeightNorm < weightNorm)
{
  double coef = maxWeightNorm / weightNorm;
  weight *= coef;
  bias *= coef;
}
{% endcodeblock %}

また、これは maxout に限りませんが、恒等関数をアクティベーション関数とするレイヤーの出力をそのまま softmax レイヤーに接続する場合、
入力が大きすぎて指数関数がオーバーフローすることがあります。

softmax のアクティベーション $\mathbf{a}$ は入力 $\mathbf{x}$ より
$$
a_i = \frac{exp(x_i)}{\sum_k exp(x_k)}
$$
で計算されますから、$\mathbf{x}$ のすべての要素から同じ数をさっぴいても結果は変わりません。
要素の max を計算し、それをすべての要素から引いてから計算することで、#INF00 を避けることができます。

**リファレンス**<br>
[1] Geoffrey E. Hinton, Nitish Srivastava, Alex Krizhevsky, Ilya Sutskever, and Ruslan R.　Salakhutdinov. Improving neural networks by preventing co-adaptation of feature detectors.
2012. arXiv:1207.0580.