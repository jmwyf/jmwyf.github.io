---
title: "Skip-gram模型（2）"
date: 2023-07-12 19:36:02
description: "Skip-gram模型pytorch和tensorflow实践"
tags: [nlp]
categories: Deep Learning
draft: false
---

{{< katex >}}

> [之前文章](https://youngforever.tech/posts/20230205-skip-gram-part1/)介绍了skip-gram的原理，这篇文章给出模型的实现细节以及pytorch和tensorflow的实现。

## 回顾
假如用余弦相似度来计算两个词的one-hot编码得到0，即不能编码词之间的相似性，所以有了word2vec的方法，包括skip-gram和CBOW。

接前文，假如我们拥有10000个单词的词汇表，想嵌入300维的词向量，那么我们的**输入-隐层权重矩阵**和**隐层-输出层的权重矩阵**都会有 10000 x 300 = 300万个权重，在如此庞大的神经网络中进行梯度下降是相当慢的。更糟糕的是，你需要大量的训练数据来调整这些权重并且避免过拟合。百万数量级的权重矩阵和亿万数量级的训练样本意味着训练这个模型将会是个灾难。[^1] 所以在具体实践上有一些计算技巧。

## 计算
skip-gram模型基于单个输入来预测上下文，对于T个训练单词$w_1,...w_T$，即最大化以下概率$$\frac{1}{T}\sum_{t=1}^T\sum_{-c<=j<=c}logP(w_{t+j}|w_t)\tag1$$其中c为训练上下文的窗口大小，$p(w_{t+j}|w_t)$用softmax来计算$$p(w_o|w_i)=\frac{exp(v_{wo}^Tv_{wi})}{\sum_{w=1}^Wexp(v_{wo}^Tv_{wi})}$$其中$v_{wi}$为输入词向量的表征，在单词量W巨大的情况下（通常都有$10^5-10^7$），式1的计算开销巨大。

在skip-gram实际算法中使用多种策略来减少模型的资源使用（内存）以及提高词向量表征质量[^2]
- 负采样
	- 从隐藏层到输出的Softmax层的计算量很大，因为要计算所有词的Softmax概率，再去找概率最大的值。例如当我们用训练样本 ( input word: "fox"，output word: "quick") 来训练我们的神经网络时，“ fox”和“quick”都是经过one-hot编码的。如果我们的vocabulary大小为10000时，在输出层，我们期望对应“quick”单词的那个神经元结点输出1，其余9999个都应该输出0。在这里，这9999个我们期望输出为0的神经元结点所对应的单词我们称为“negative” word。**当使用负采样时，我们将随机选择一小部分的negative words（比如选5个negative words）来更新对应的权重**, 我们也会对我们的“positive” word进行权重更新[^3]。在实践中，​通常使用的是unigram分布的平方根，​即词汇表中每个词的概率的0.75次方除以归一化常数来挑选负样本。
- 高频词进行抽样
	- 原因：高频词相对于低频词来说提供的信息少；高频词随着样本增多本身表示也不会发生太大变化
	- 使用概率P来丢掉一定的单词$$P(w)=1- \sqrt{\frac{t}{f(w_i)}}$$其中t为设定的阈值，$f(w_i)$为单词出现的频率，可以看到频率越高丢弃的概率越大，反之越小
- 单词组合成词组作为单个词处理
	- 原因：组合词有特定的意思，不是简单把单个词的表示聚合起来
	- 如何从文本中提取出词组研究不少，skip-gram文章选用了$$socre(w_i, w_j)=\frac{count(w_iw_j)-\delta}{count(w_i)*count(w_j)}$$其中$w_i, w_j$代表不同的单词，利用score得分与设定的阈值比较来确定是否为常见词组

## skip-gram PyTorch实现
Word2vec skip-gram pytorch[^4]

[skipgram-pytorch.ipynb](https://github.com/yongfanbeta/skip-gram/blob/main/skipgram-pytorch.ipynb)

## skip-gram Tensorflow实现
Word2vec skip-gram tensorflow[^5]

[skipgram-tf.ipynb](https://github.com/yongfanbeta/skip-gram/blob/main/skipgram-tf.ipynb)

[^1]: [理解 Word2Vec 之 Skip-Gram 模型 - 知乎](https://zhuanlan.zhihu.com/p/27234078)
[^2]: [Distributed Representations of Words and Phrases and their Compositionality](https://arxiv.org/pdf/1310.4546.pdf)
[^3]: [关于skip-gram和负采样 - 简书](https://www.jianshu.com/p/cf36d4c1ea39)
[^4]: [14.1. 词嵌入（word2vec） — 动手学深度学习 2.0.0 documentation](http://zh.d2l.ai/chapter_natural-language-processing-pretraining/word2vec.html)
[^5]: [word2vec  |  TensorFlow Core](https://www.tensorflow.org/tutorials/text/word2vec#negative_sampling_for_one_skip-gram)


