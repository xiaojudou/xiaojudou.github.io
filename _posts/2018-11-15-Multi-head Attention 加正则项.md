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

Attention 机制的几个部分：

* Q,K,V: Q 来自 query，K 和 V 是 document 的两个值，V 是本身的  value，K 是用来和 Q 算 attention 的。实际经常 K 就直接取 V 的值了，self-attention 的时候 Q,K,V 是同一个东西。
* A：Attention 值，是 Q 和 K 相乘 or 过一个 feed-forward 算出来的。
* O：output，A 和 V 的乘积。
* H：head 的个数

第一种正则：让 document 的不同表示（H 个不同的 V）尽量正交。

$
D_{ \text {subpace} } = - \frac { 1 } { H ^ { 2 } } \sum_{ i = 1 } ^ { H } \sum_{ j = 1 } ^ { H } \frac { V ^ { i } \cdot V ^ { j } } { \left\| V ^ { i } \right\| \left\| V ^ { j } \right\| }
$

第二种正则：希望 attention 注意的位置不同。

$
D_{ \text {position} } = - \frac { 1 } { H ^ { 2 } } \sum_{ i = 1 } ^ { H } \sum_{ j = 1 } ^ { H } \left| A ^ { i } \odot A ^ { j } \right|
$

第三种正则：让 output 尽量正交，这样可以表示不想关的含义。

$
D_{ \text {output} } = - \frac { 1 } { H ^ { 2 } } \sum_{ i = 1 } ^ { H } \sum_{ j = 1 } ^ { H } \frac { O ^ { i } \cdot O ^ { j } } { \left\| O ^ { i } \right\| \left\| O ^ { j } \right\| }
$

最后的结果比 《attention is all you need》在英德翻译等上面好了很多。

经过分析，第二种正则也就是强制不同 head 的 attention 位置不同对结果没有决定性的影响。
猜测有以下三个原因：
* attention 本身就应该注意到重点的词上面。
* A 是由 K 和 Q 得来的，这种正则约束使得 element-wise 的乘积和最小，而不能要求每个位置尽量有 0。
* 《ALL》里面用的是 position encoding，是相对于位置的固定值，可能会有干扰。
