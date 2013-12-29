---
layout: post
title: "初ポスト"
date: 2013-12-29 21:03:36 +0900
comments: true
categories: 
---
二日がかりでようやく Github + Octopress のサイトを立ち上げました。
<!-- more -->

数式もかけます。
$$
P(E)   = {n \choose k} p^k (1-p)^{ n-k}
$$

Solarized によるソースコードの表示。
{% codeblock lang:cpp %}
// For each sample (=row) in the input,
// 'dropoutRatio' by ratio of columns shall be set to zero.
static void Dropout(cv::Mat &inputs, cv::Mat &dropped, const real &dropoutRatio)
{
  cv::Mat mask(inputs.size(), CV_8U);
  cv::RNG rng(time(NULL));
  makeDropoutMask(mask, dropoutRatio, rng);

  inputs.copyTo(dropped, mask);
}
{% endcodeblock %}

###参考にしたサイト
- まずは[Octopress 公式ドキュメント](http://octopress.org/docs/)
- 当方 OS が Windows 7 のため、[このサイト](http://stb.techelex.com/setup-octopress-on-windows7/)の手順に従ってインストールしました。実は一度インストールしてから変になってしまって、Cygwin を使う方法に変更して再構築をかけたのですが泥沼にはまり、結局、再びこちらの方法に戻って現在に到っております。
- [mathy というテーマ](https://github.com/stchangg/mathy)は MathJax という仕組みをサポートしていてい、上のように数式を表示できるようになります。これを元に色々カスタマイズしました。
- 静的サイトのジェネレーター[Jekyll のドキュメント](http://jekyllrb.com/docs/home/)。octopress/sourceにユーザーが用意したファイルを元に、Jekyll が octopress/public にサイトのファイル一式を作ってくれる。Jekyll を理解しないとサイトのカスタマイズができない。

その他にも多くのサイトを参考にしましたが、ほとんどは上記のリンクからたどって行けます。

###はまった点
- Github のプロファイルは、少なくとも名前と Email は前もって埋めておく。
- rake setup_github_pages でレポジトリの url を尋ねられるが、 git@... の方ではなく https://... の方にしておくと SSH キーを作る必要がなくなるのでおすすめ。
- コンソールに表示されるメッセージはよく読もう！実はエラーが起きているのに気づかなかったことがしばしば。
- markdown ファイルで日本語を使うときは BOM なしの UTF-8 で保存する。編集には Notepad++ を使うと便利。
- Solarized を使うには Python が必要となるので、Windows 用の Python を必要に応じてインストールし、Git Bash のパスに追加する必要がある。~/.bash_profile に export PATH="/c/Ruby193/bin:/c/Python27:$PATH" という風に書いておく。
