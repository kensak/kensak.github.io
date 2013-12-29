---
layout: post
title: "２番目のポスト（テスト）"
date: 2013-12-29 12:03:36 +0900
comments: true
categories: 
---
最初のポスト。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。
<!-- more -->

続きの行。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。何か長い行をここにかきます。


数式の例。
$$
P(E)   = {n \choose k} p^k (1-p)^{ n-k}
$$

コードの例。

    // For each sample (=row) in the input, 'dropoutRatio' by ratio of columns shall be set to zero.
    static void Dropout(cv::Mat &inputs, cv::Mat &dropped, const real &dropoutRatio)
    {
      cv::Mat mask(inputs.size(), CV_8U);
	  cv::RNG rng(time(NULL));
      makeDropoutMask(mask, dropoutRatio, rng);

      inputs.copyTo(dropped, mask);
    }
