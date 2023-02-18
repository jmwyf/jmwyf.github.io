---
title: "公开重症监护数据库MIMIC代码仓库介绍"
date: 2023-02-04 10:57:02
description: "《The MIMIC Code Repository: Enabling reproducibility in critical care research》论文"
tags: ["medical data", "mimic-iv"]
categories: Paper
showSummary: true
math: true
---

{{< lead >}}
《The MIMIC Code Repository: Enabling reproducibility in critical care research》论文
{{< /lead >}}

## 引言
- 科学结果的可重复性越来越受到关注[^1]；
- 医疗领域进入数字化革命（本文是2017年接收），引出形成MIMIC-III数据库；
- EHR二次分析需要临床专家和数据科学家的合作，在EHR数据库上推导或者定义一些概念是需要资源的，对于没有特别强的临床背景或者数据科学技能的人来说巨大障碍；
- 该文介绍MIMIC代码仓库，介绍与重症相关概念的导出以及相关假设条件等；
- 公开数据已经逐渐有了，公开相应的数据代码同样重要。加速并提升未来研究的一致性以及有效性。

## 代码仓库详情
- Concepts
	- 从电子病历中提取重要概念的代码。比如提取AKI的模块
- Executable documents
	- 可执行的Notebooks文件，可重复的示例研究或者教程
- Community
	- 建立公开讨论便于社区成员贡献

### 概念concepts
代码库中常用的概念
#### 疾病严重程度评分Severity of illness scores
在回顾性数据库中难以计算
- 大多都是在前瞻性实验中获取的；
- 常规收集的数据缺相应元素。有些特征未纳入结构化电子病历系统，另外则是对某种情况的患者没有统一的协议来定义状态

目前MIMIC代码库中有：
- acute physiology score(APS)-III
- simplified acute physiology score(SAPS)
- SAPS-II
- Oxford acute severity of illness score(OASIS)

#### 器官衰竭Organ dysfunction scores
SOFA计算方式不同，由于GCS评分定义不同

Sequential Organ Failure Assessment(SOFA), Logistic Organ Dysfunction system(LODS)

#### 治疗时间Time of treatment
由于数据获取的限制，许多药物和确切的治疗时间无法得出，需要根据临床经验识别其他可替代的数据
- 机械通气时长：识别机械通气时长需要复杂的逻辑规则（文中图3）
- 血管加压药物使用
- CRRT

#### 脓毒症sepsis
sepsis定义有多种版本，这里给出了*Angus 2001，Martin 2003，Iwashyna 2014*三个版本

#### 共病Comorbidities
给出了4个版本
- *Elixhauser A 1998*
- American Health and Research Quality group（AHRQ）
- *Quan 2005*
- *Van Walraven 2009*

#### concept指南
![](https://cdn.jsdelivr.net/gh/jmwyf/pichosting@master/mimicroadmap.png)

### 可执行文档
当数据和代码都公开可获取，提供一种研究可以被重现的框架，基于Rmd或notebook给出实例。
- *Hsu 2015*研究复现
	- indwelling arterial catheters and their association with in-hospital mortality for hemodynamically stable patients with respiratory failure
	- `aline.ipynb`提取数据
	- `aline.Rmd`数据分析
- 教程
	- definition of CRRT
	- introduction to SQL
	- a step-by-step guide to selecting a study cohort
	- an outline of the data-capture process

### 社区
让研究人员和数据维护人员、临床人员共同提升代码

## 结论
公开数据库的案例已经不少，为了让研究更加透明，也需要公开相应数据分析和数据处理的代码

## 补充
- 代码库地址：https://github.com/MIT-LCP/mimic-code
	- 之前以MIMIC-III为主，现在mimic-iii和mimic-iv合并在一起了
- mimic数据库为了让研究者访问更加方便，很大一个改变是部署在云上比如google的云平台，云平台上需要big query语法来访问，所以现在代码库关于数据提取的代码更新以big query为主，需要通过脚本转化为适合postgres语法
	- Open a terminal in the `concepts` folder.
	- Run [convert_bigquery_to_postgres.sh](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iv/concepts/convert_bigquery_to_postgres.sh).
		-   e.g. `bash convert_bigquery_to_postgres.sh`
		-   This file outputs the scripts to the [postgres](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iv/concepts/postgres) subfolder after applying a few changes.
		-   This also creates the `postgres_make_concepts.sql` script in the postgres subfolder.
- 从代码仓库导出的概念concepts都放到`mimic_derived`数据集里

[^1]: Johnson, A. E. W., Stone, D. J., Celi, L. A. & Pollard, T. J. The MIMIC Code Repository: enabling reproducibility in critical care research. _J Am Med Inform Assn_ **25**, 32–39 (2018).