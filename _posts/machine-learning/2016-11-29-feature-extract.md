---
author: voidleaf
comments: true
date: 2016-11-28 18:13:10+00:00
layout: post
title: 特征选择方法简介
categories:
- 机器学习
post_format:
- 日志
tags:
- python
- sklearn
- 随机森林
- 数据分析
- 机器学习
---

## 特征选择的步骤

1. 子集搜索
2. 子集评价

<!--more-->

## 常见的特征选择方法

### 过滤式选择

过滤式选择采用先对数据集进行选择，然后再训练学习器，特征选择过程与后续的学习器无关。常见的算法是Relief (Relevant Features)算法。

#### Relief算法介绍

Relief (Relevant Features)[Kira and Rendall, 1992] 算法是一种特征权值方法，Relief算法对于每一个特征 i，每次随机选取一个样本 x，计算 x 与其猜中近邻 h（x 的同类样本中与 x 距离最近的样本）与猜错近邻 k（x 的异类样本中与 x 距离最近的样本）之间在特征 i 上的距离，得到 -Diff( i, x, h) + Diff (i, x, k),  重复 m 次之后得到的平均值作为对该特征效用的评估，如果值比较大就说明该特征对不同分类的样本有着很好的区分度，反之则说明特征的分类能力较弱。如果值小于某一阈值，则可以将该特征删除，Relief算法的时间复杂度随特征数量线性增长，因此效率非常高。

Relief 算法的伪代码如下

```
#A表示特征集合
#m表示随机选取的样本数量
#D表示数据集
#W表示权值
for I in A
	W = 0
	for j = 1：m
		x = randomSelect(D)
		h = nearestSameKind(x)
		k = nearestDiffKind(x)
		W = W + (- Diff(I, x, h) + Diff(I, x, k))/m
	end
	if(W < theta)
		dropFeature(I)
	end
end
```

**Relief-F 算法**

Relief算法比较简单，效率也很高，但其局限性在于只能处理二分类的情况，因此1994年Kononeill对其进行了扩展，提出了 ReliefF 算法，可以处理多类别问题。该算法用于处理目标属性为连续值的回归问题。ReliefF算法在处理多类问题时，每次从训练样本集中随机取出一个样本 x，然后从和R同类的样本集中找出R的k个近邻样本(near Hits)，从每个R的不同类的样本集中均找出k个近邻样本(near Misses)，然后更新每个特征的权重

### 包裹式选择

包裹式选择直接将最终的学习器性能作为评价准则，直接针对给定的学习器进行优化，常见的算法是 LVW（Las Vegas Wrapper）[Liu and Setiono, 1996]，该算法使用随机策略进行子集搜索，以分类器的误差作为评价准则

### 嵌入式选择

嵌入式选择将特征选择的过程和学习的过程融为一体，在学习过程中自动进行特征选择，常用的方法有 LASSO 回归
