---
title: "医学人工智能周刊7｜医疗人工智能算法的公平性"
date: 2024-10-28 17:53:00
tags: [artificial intelligence, healthcare]
categories: AI4H
---

# 医疗人工智能算法的公平性
医疗领域，发展和部署不公平的人工智能系统会破坏公平治疗[^1]。在各亚人群中对AI模型的评估揭露了患者诊断、治疗以及收费上的不公平性。本文中作者从医疗角度概述了机器学习公平性，并且讨论算法偏差（特别是在数据获取、变异以及内在标签变异性）如何在临床工作流中出现并导致医疗差异。同样回顾了通过去纠缠、联邦学习和模型可解释性来减轻偏差的新兴的技术，以及这些方法在发展基于AI软件作为医疗设备的作用。

## 解决医疗差异和不平等的意义
医疗差异不平等的原因，有观察到和隐藏的危险因素，了解这些差异的根源将指导改善服务患者不足的医疗保健的决策政策。
基于含有混淆历史偏差的数据发展的算法可能造成差异伤害，评估和减轻算法造成的伤害是研究医疗机器学习的重要动力。
### 医疗差异
- 遗传变异和群体特异性表型：人种
- 社会因素：地位财富等
- 医学概念变化：ICD编码
- 数据获取变化：数据获取的设备变化
- 未知的医疗疾病：covid
- 不同发展水平的国家部署AI-based software as medical device

## 医疗公平衡量指标定义
### Demographic parity
以关注的因素如种族划分亚组，在各亚组中模型阳性预测比例相等。
### Predictive parity
阳性预测值（PPVs）和阴性预测值在各亚组应该相等。
### Equalized odds
TPRs和FPRs在各亚组相等。

## 消除医疗AI不公开的技术
通过**预处理**步骤调整现有算法，以盲化、增强或重新加权输入空间；
- importance weighting

为了消除混淆因素的影响，**处理中**技术在模型中构建了一个非区分项，来惩罚学习保护特性中的区分特征；
- non-discrimination term

**后处理**通过改变模型的输出来满足公平性指标；
- pick thresholds for each group
- a calibration curve can be fitted for each group

[^1]: Chen R J, Wang J J, Williamson D F K, et al. Algorithmic fairness in artificial intelligence for medicine and healthcare[J]. Nature Biomedical Engineering, 2023, 7(6): 719-742.



