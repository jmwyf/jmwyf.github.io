---
title: "MIT 6.S91 Introduction Deep Learning Notes"
date: 2023-01-13 16:59:00
description: "MIT 6.S91 Introduction Deep Learning（2022）笔记"
tags: [deep learning]
categories: Class
math: true
---

{{< katex >}}

## 1.Introduction to Deep learning
- 震撼，第一节课直接放大招，用自己拍摄的视频和奥巴马合成来介绍这门课程。
- **不管老师在课程上讲什么，希望你们能真正的思考为什么这一步是重要而且必须的，正是这些思考才能做出真正令人惊讶的突破。**

## 2.Deep Sequence Model
Three way to solve gradient vanish 
* Gated Cells
	* LSTM
		* Forget
		* Store
		* Update
		* Output
* Attention [[Transformer]]

## 3.Deep Computer Vision
- 介绍卷积操作，是一种提取特征的方法生成feature maps（还有其他的方法可以用吗？然后效果还不错）；
	- 与全连接相比的优点；
- [ ] Fast RCNN用于目标检测，怎么实现推荐特定区域图像？
* 医学图片分割
- 总结：
	* 原理
	* CNN架构
	* 应用

## 4.Deep Generative Models
- what 目标: 来自于一些分布中的训练样本，通过这些样本学习模型来表征这个分布；
- how 密度估计；神经网络适合来进行高维度表征；
- why 
	* **Debiasing**: Capable of uncovering underlying features in a dataset
	* Outlier detection: how can we detect when we encounter something new or rare?
* Latent variable representation:
	* 举例事物的投影，只能看见影子即表象，而被灯光照射的实物是看不见的即隐变量；要做的是通过观察到的投影来对实物进行建模
* Autoencoder: reconstruction loss
	* 完全是确定性性
- VAEs：normal prior + regularization
	- reconstruction loss + regularization term
	- encoder: $q_\phi(z|x)$ 
	- decoder: $p_\theta(x|z)$
	- KL-divergence: $D(q_\phi(z|x)||p(z))$ 

![](https://cdn.jsdelivr.net/gh/jmwyf/pichosting@master/VAEsummary.png)

- GANs
	- make a generative model by having two neural networks compete with each other
	- ⭐️CycleGAN: domain transformations 视频开头的视频就是用这个合成

## 5.Deep reinforcement learning
- Reward: $$R_t = r_t + \gamma r_{t+1} + \gamma^2 r_{t+2} + ...$$
- Q-function: expected total future reward $$Q(s_t, a_t) = E[R_t|s_t, a_t]$$
- Policy: to infer the best action to take at its state, choose an action that maximizes future reward $$\pi^*(s)=\mathop{\arg\max}\limits_{s}Q(s, a)$$
- Value Learning
	- find $Q(s, a)$
	- $a = \mathop{\arg\max}\limits_{a}Q(s, a)$
- Police Learning
	- find $\pi(s)$
	- sample $a\sim\pi(s)$
- [ ] Deep Q Network(DQN)
- [ ] Policy Gradient
- [ ] AlphaGo

## 6.DL Limitations and New Frontiers
- limitations
	- Generalization
		- data is important
	- Uncertainty in Deep learning
	- adversarial attack
	- Algorithmic Bias
- Frontiers
	- encoder
		- many real world data cannot be captured by standard encodings
		- [ ] GCN（Graph Convolutional Networks）
	- Automated AI

## 7. LiDAR for Autonomous Driving
@INNOVIZ
- Camera Vs LiDAR
	- 互补，视线不好的情况
	- 冗余能保证准确
- Safety and Comfort

## 8. Automatic Speech Recognition
@Rev 
- [ ] Conformer
- [ ] CTC

## 9. AI fore Science
Principled AI Algorithms for challenging domains
@Caltech

## 10. Uncertainty in Deep Learning
longer version：NeurIPS 2020 Tutorial
@Google AI Brain Team
- Return a distribution over predictions rather than a single prediction 
- Out-of-Distribution Robustness
	- covariate shift: distribution of features changes
	- open-set recognition: new classes may appear at test time
	- label shift: distribution of label changes
- sources of uncertainty
	- Model uncertainty
		- 认知上的不确定性
	- Data uncertainty
		- human disagreement label noise
		- measurement noise
		- missing data
- how to compute
	- [ ] BDN
	- [ ] GP
	- [ ] Deep Ensemble
	- [ ] MCMC
- multi-input and multi output（MIMO）
- how to communicate with uncertainty?

7-10讲很一般，一个复杂的主题，需要将背景讲清楚，公司讲东西也没啥具体细节。


## Ref
1. [【双语字幕】MIT《深度学习导论(6.S191)》课程(2021)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1jo4y1d7R6/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=c2e29329f33c2e7eb04916d212234ad6)
2. [introtodeeplearning.com](http://introtodeeplearning.com/)
3. [MIT 6.S191: Deep Generative Modeling - YouTube](https://www.youtube.com/watch?v=QcLlc9lj2hk&list=PLtBw6njQRU-rwp5__7C0oIVt26ZgjG9NI&index=4)