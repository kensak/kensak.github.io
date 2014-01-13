---
layout: post
title: "ランダムな置換とクヌース・シャッフル"
date: 2014-01-13 19:52:19 +0900
comments: true
categories: [アルゴリズム, C++]
---

ニューラルネットをプログラムするときに、ランダムな置換
（≒ 0 から n-1 までの数がランダムな順で並んだもの）が
必要になることがあります。例えば、

+ SGD with minibatch において、サンプルの中からランダムな順序で 100 ずつ、
順番に取り出したい。
+ Dropout において、n 個の入力のうちランダムに選んだ半分を 0 にセットしたい。
（それぞれの入力を確率 0.5 で 0 にするのではちょうど半分になるとは限らない。）

これには [Knuth shuffles](http://en.wikipedia.org/wiki/Random_permutation)
という便利なアルゴリズムがあります。

<!-- more -->

{% codeblock lang:cpp %}

// get a random permutation of 0, 1, .., len-1]] where len = vec.size().
void GetRandomPermutation(std::vector<unsigned int> &vec)
{
  // Knuth shuffles algorithm
  const size_t len = vec.size();
  for (unsigned int i = 0; i < len; ++i)
  {
    unsigned int j = uniform(0, i);
    vec[i] = vec[j];
    vec[j] = i;
  }
}

{% endcodeblock %}

ここで `uniform(0, i)` は $0, 1, \ldots,i$ の中から同確率で数を一つ選んで
返す関数だとします。

このアルゴリズムは要するに $i$ 回目のループで、まず `vec[i]` に $i$ を入れ、
さらに $[0, i]$ からランダムに $j$ を選んで `vec[i]` と `vec[j]` の値を入れ替えているだけです。

（9 行目で $i = j$ の場合、まだ初期化されていない値を自分に代入することになりますが、
そのときは次の 10 行目で $i$ が上書きされるので問題ありません。）

このように簡単なアルゴリズムですが、
これで最終的に得られる置換の分布は一様であることが、次のように証明できます。

数学的帰納法を使います。  

> $k$ 番目のループのあと、vec[p] $(p \in [0,k])$ の値が $q \in [0, k]$ である確率は $p, q$
に関わらずすべて $1 / (k + 1)$ である

という命題を $P(k)$ とします。  
$P(0)$ は自明です。  
$P(k)$ を真だと仮定して $P(k+1)$ を証明すればいいわけですが、$k+1$ 番目のループのあと、

+ `vec[p]` $(p \in [0,k])$ が $q \in [0, k]$ である確率は、
「前回のループ後にその値であった確率」×「今回入れ替えられなかった確率」、
つまり
$$
(1 / (k + 1)) \cdot ((k + 1) / (k + 2)) = 1 / (k + 2)
$$
+ `vec[p]` $(p \in [0,k])$ が $k + 1$ である確率は、今回入れ替えられた確率、
つまり $1 / (k + 2)$
+ `vec[k+1]` が $q \in [0, k+1]$　である確率はやはり $1 / (k + 2)$

というわけで  $P(k+1)$ が成り立ちます。




