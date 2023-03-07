---
title: "MultiBench多模态表征学习的多尺度基准"
date: 2023-03-04 15:17:00
summary: "Multiscale Benchmarks for Multimodal Representation Learning论文解读"
tags: [multimodal, mimic-iii]
categories: Deep Learning
showSummary: true
---

MULTIBENCH，一个系统而统一的大规模多模态学习基准，涵盖15个数据集、10种模式、20个预测任务和6个研究领域[^1]。

## 引言
背景：
- 语言和视觉领域多模态学习发展不错，但是其他领域欠缺
- 现在的基准评价关注性能，没有量化缺点包括时间空间复杂度，由于不完美模态导致的鲁棒性降低，需要在性能、鲁棒性、复杂度取得平衡

提出multibench就是解决以上问题：
- 扩充收集各领域数据集、数据模态
- 量化复杂度
- 提出标准流程评价对噪声和缺失模态情况下的鲁棒性

MultiBench是一个端到端的过程，包括数据预处理、数据集拆分、多模态算法、评估指标和交叉验证。
- 开发工具包MultiZoo
- 可以用于workshop、教学等

## 多尺度多模态基准
第一版集中在多模态融合，对于多模态翻译等问题未来版本可能涉及

### 数据集
介绍了6大领域15个数据集，表1
- 情感计算（affective computing）
- 医疗：时变和静态变量的整合使用
- 机器人
- 金融
- 人机交互
- 多媒体
![](https://cdn.jsdelivr.net/gh/jmwyf/pichosting@master/multibenchdata.png)

### 评价标准
性能： 
- regression: MSE, MAE,
- classification: F1-score, AUPRC

复杂度：
- data size in bits
- number of model parameters
- time and memory resources on CPU and GPU

鲁棒性：
- 单模态独有噪音：对图像、音频等单独处理
- 考虑多模态整体的不完善：比如缺失模态等

## MultiZoo：多模态算法集合
涵盖实现multibench整个过程中的算法

### 数据预处理
- WordAlign算法
	- 将各模态信息调整到统一粒度

### 融合范式
- 早期和晚期融合
	- EF，LF
- 多模态张量: 多模态互补
	- Tensor Fusion
	- Low-rank Tensor Fusion
- 多模态乘法交互: 多模态交互
	- MI-MATRIX
	- MI-VECTOR
	- MI-SCALAR
- 多模态门控
	- NL GATE: 自注意力机制
- 时序注意力模型
	- MULT: 多模态Transformer
- 网络架构搜索
	- MFAS

### 优化目标
除了标准的监督损失函数，纳入一些新提出的目标函数
- CCA
- REFNET
- MFM
- MCTN

### 训练过程
- Gradient Blending来计算融合的权重
- Regularization by Maximizing Functional Entropies

## 实验
- 泛化性能
	- 目前的方法表现出高方差，没有放之四海而皆准的模型，特别是对于未被研究的模式和任务。
	- 后期融合表现比较均衡
	- 有些融合方法是专门为2模态设计，有些在2/3模态表现不好
- 单模态与多模态的权衡
- 性能与复杂度的权衡
- 性能与鲁棒性的权衡

## 结论
一个大规模的基准，统一了以前在多模态研究中互不相干的工作，重点是易用性、可及性和可重复性。

### 未来拓展
- 其他的多模态问题
- 新的评价指标
- 多模态迁移学习或者协同学习
- 多模态多任务学习

## 思考
MultiBench把以前多模态研究中使用的公开数据集，算法，评价指标等都统一在了一个框架下，期望标准化多模态学习过程，并且能将不同的算法模型在其他模态、任务中进行比较。大而全的框架确实能为各类多模态任务提供一个baseline，但是各专业领域内的多模态模型应该是存在一些差异的，就像我们很难期待一个医生能掌握律师干的事情，然而，人工智能的发展确实很快，比人还强大的通用人工智能应该也会实现。

[^1]: Liang, P. P. et al. MultiBench: Multiscale Benchmarks for Multimodal Representation Learning. (2021) doi:10.48550/arXiv.2107.07502.
