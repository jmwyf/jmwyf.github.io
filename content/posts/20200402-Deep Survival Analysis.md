---
title: "Deep Survival Analysis"
date: 2020-04-02 10:58:16
description: ""
tags: []
featured_image: "/images/"
categories: Paper
comment : true
---

# 深度生存分析(Deep Survival Analysis)
电子健康档案(electronic health record;EHR)为建立可行的工具来辅助医生提供史无前例的机会。

## 1. 引言
目的：利用EHR来估计感兴趣事件未来发生的时间，即医疗领域的生存分析。

基于患者疾病严重风险的管理治疗在临床决策过程存在的挑战十分普遍。列举了传统疾病评分的三个挑战：
- 需要完整的数据
- 患者EHR数据依据统一的标准统一队列（感觉EHR没这个问题，作者想表达的可能是比较难标准化）
- 协变量与结局变量假设线性，可能存在交互项等

本文提出基于EHR生存分析的新模型，该工作主要几个贡献：
- 将协变量和生存时间在贝叶斯框架下进行建模，简化缺失值处理
- Deep exponential families(Ranganath et al., 2015b)，一种深层隐变量模型，能捕捉协变量与失效时间复杂的依赖关系
- 根据失效时间来对所有患者进行标准化而非强制执行人工的时间零点校准
- EHR数据优秀的预处理让模型能输入多维数据

本文安排：
- 介绍传统的生存分析以及对于EHR数据需要更好的模型的需求
- 介绍Deep exponential families; 讨论模型标准化策略以及模型假设
- 拓展性好的变分推理算法
- 数据、实验设计、评估方法
- 结果结论
  
## 2. 生存分析
### 2.1 传统的生存分析
- Kaplan-Meier estimator，即k-m曲线

    生存函数的非参数估计，1-累计分布函数
- Cox比例风险模型

    半参数模型，能包括协变量
最后提到这些方法的贝叶斯变种讲先验概率加到对参数估计上。

### 2.2 应用EHR数据传统生存分析的局限
- EHR数据高维，稀疏，传统建模方法不太容易处理缺失值
- 传统建模需要基于一个同步事件（进入临床试验，干预时间等）来标准化所有患者数据。该方法能评估任何时间的风险不必基于同步事件
- 基于回归的方法假设线性关系，该方法能处理协变量与事件间的非线性关系
  
## 3. 深度生存分析
层次生成方法（a hierarchical generative approach for survival analysis）

### 3.1 Deep Exponential Families(DEF)
DEF深度指数族(DEF)是一类由指数族建立的多层概率模型。

### 3.2 通过失效对齐
需要了解两中常用的分布：
- 指数分布


- Weibull分布：To model time-to-failure data
$$f(x)=ax^{k-1}exp(-(bx)^k)$$
根据概率归一性，可以得到
$$a = kb^k$$
所以，标准形式也写作：
$$f(x;\lambda,k)= 
\begin{cases}
\frac{k}{\lambda}(\frac{x}{\lambda})^{k-1}e^{-(x/\lambda)^k}&\text(x≥0)\\\\
0&\text(x<0) 
\end{cases}$$
k-`shape parameter`形状参数
$\lambda$-`scale parameter`范围参数


## ref
1. [Deep Exponential Families](https://arxiv.org/pdf/1411.2581.pdf)