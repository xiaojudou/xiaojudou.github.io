---
layout: post
title: "Multi-head Attention 加正则项优化"
description: "Test post for style"
date: 2018-11-15
tags: [attention, EMNLP, 2018]
comments: true
share: true
---

[EMNLP2018] Multi-Head Attention with Disagreement Regularization

Multi-head attention 是并行地去进行多次 attention，最后把结果拼起来的过程，它的好处在于它使不同的 head 去形成不同的表示。但它的缺点是并没有机制能够确保这些 head 注意的东西是不一样的。所以这里加了正则项 disagreement regularization 去强制它们不同。

文章一共提了三种不同的正则项，可以组合地加在目标函数的后面。
