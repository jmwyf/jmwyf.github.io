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

## 引言
目的：利用EHR来估计感兴趣事件未来发生的时间，即医疗领域的生存分析。

基于患者疾病严重风险的管理治疗在临床决策过程存在的挑战十分普遍。列举了传统疾病评分的三个挑战：
- 需要完整的数据
- 患者EHR数据依据统一的标准统一队列（感觉EHR没这个问题，作者想表达的可能是比较难标准化）
- 协变量与结局变量假设线性，可能存在交互项等

本文提出基于EHR生存分析的新模型：
- 将协变量和生存时间在贝叶斯框架下进行建模，简化缺失值处理
- 
