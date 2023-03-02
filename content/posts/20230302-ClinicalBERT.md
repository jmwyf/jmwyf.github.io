---
title: "ClinicalBERT: 对医学文本建模用于再入院预测"
date: 2023-02-21 23:13:00
summary: "ClinicalBERT Modeling Clinical Notes and Predicting Hospital Readmission论文解读"
tags: [nlp, bert, mimic-iii]
categories: Deep Learning
showSummary: true
---

{{<katex>}}

使用临床文本预训练BERT然后在再入院任务中微调

## 引言
非结构化、高维稀疏信息例如临床文本难以在临床机器学习模型中使用。临床文本中包含什么样的临床价值？更加丰富、详细。然而重症监护室医生在有限时间内需要做出最优决策，读大量的临床文本，增加工作量。

再入院会降低患者生活质量、增加花费。这篇文章旨在发展一个出院决策模型，根据医护人员笔记动态的赋予患者30天再入院的风险。

### 背景
临床文本会有缩写、黑话、不标准的语法结构，从临床文本中学习有用的表征具有挑战。以往的方法无法捕捉获取临床意义的文本长程依赖，介绍BERT，以及用BERT已经开展的工作，已经有人把BERT用在临床文本了，本文在再入院任务上评估改进ClinicalBERT并且在更长的序列上进行预训练。

介绍前人在ICU再入院预测上的工作，缺点：大多数工作都只用了出院的信息，ClinicalBERT使用患者住院整个时间段信息。

### 该工作的重要性：
- 用出院信息来预测意味着减少了再入院风险的机会少了，都要出院了，此刻告诉有再入院的风险，难以采取措施
- 由于很多误报警，医疗模型需要高PPV[^1]，该模型最高的recall[^2]
- 模型中attention能用于可视化解释

## 方法 
### 什么是BERT
BERT是基于transformer编码器架构的深度神经网络，它用于学习文本的嵌入表达。
- 自注意力机制
- BERT模型通过2个无监督任务进行预训练：掩码模型和下一个句子预测。

### 临床文本嵌入
- 先分词成token[^3]，这里是子词粒度的tokenization[^4]
- ClinicalBert的token包括子词、分段嵌入、位置嵌入
	- 分段嵌入是当多个序列输入时，表示当前的token属于哪一段
	- 位置嵌入即在输入序列中token的位置

### 自注意力机制
用于输入token之间的关系捕捉

### 预训练
BERT是在BooksCorpus和Wikipedia中预训练的，临床文本黑话缩写，与一般文本可能语法也不一样，需要在临床文本中进行预训练。损失函数是预测掩码单词任务和预测两个句子是否连续任务损失函数之和。

### 微调
在再入院任务中微调
$$P(readmit = 1 | h_{[cls]}) = \sigma(Wh_{[cls]})$$
式中W为参数，h为BERT模型输出。

## 实验
### 数据
MIMIC-III中2083180份去隐私化后的文本，五折每一轮其中四折预训练，最后一折微调

### 实证研究I
- 在临床语言建模中ClinicalBERT与BERT进行比较：预测掩码token以及2个句子是否连续任务中均优于BERT
- 定性分析：专家给出相似医学概念，ClinicalBERT学习嵌入表达后，进行降维可视化，发现相近
- 定量分析：采用相似度度量公式计算表征之前相似度，然后与专家打分的相似度进行关联分析计算pearson相关系数

### 实证研究II
- 再入院队列：34560患者，2963再入院，42358负样本，*这里为啥有这么多负样本？*
- 调整后的再入院预测：
  $$P(readmit = 1|h_{patient}) = \frac{P^n_{max}+P^n_{mean}n/c}{1+n/c}$$
	- 有些文本是比较重要，有些文本对再入院预测不重要，所以要包括最大的概率
	- 噪声会降低性能，消除噪音的方法还是取大多数值的平均，如果序列越长，噪声出现的可能性越大，所以需要平均值的权重越大，引入了n/c作为比例因子
	- 分母则是用于概率归一化到0,1区间
- 评估指标
	- AUROC
	- AUPRC
	- RP80：准确度为80%时候到召回率
- 模型比较：Bag of words，BI-LSTM，BERT
- 用出院记录来进行再入院预测
- 用24-48小时数据预测，以及48-72小时数据预测
- 可解释性
	- 给出一句话的self-attention权重示意图

## 讨论
建议在私有数据集上重新训练后在下游任务中使用

## 代码
[GitHub - kexinhuang12345/clinicalBERT: ClinicalBERT: Modeling Clinical Notes and Predicting Hospital Readmission (CHIL 2020 Workshop)](https://github.com/kexinhuang12345/clinicalBERT)

## 思考
自chatgpt后，大型语言模型受到广泛关注，医学语言模型的发展似乎有多种路径，一种是直接在通用文本上预训练，一种是在医学文本中预训练，或是通用模型在领域微调，个人感觉应该是第三种效果会较好。

[^1]: Huang, K., Altosaar, J. & Ranganath, R. ClinicalBERT: Modeling Clinical Notes and Predicting Hospital Readmission. in _CHIL_ (arXiv, 2020). doi:[10.48550/arXiv.1904.05342](https://doi.org/10.48550/arXiv.1904.05342).
[^2]: PPV: 阳性预测里面真正的阳性比例 
[^3]: recall: 正样本中实际预测为正，即真阳性率
[^4]: token：将原始文本切分成子单元的过程就叫做Tokenization，子单元即token
[^5]: [NLP中的Tokenization - 知乎](https://zhuanlan.zhihu.com/p/444774532?utm_id=0)
