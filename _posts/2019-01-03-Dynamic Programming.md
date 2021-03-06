---
layout:	post
title:	"Dynamic Programming"
image:	''
date:	2019-01-03 09:14:52
tags:	
- Algorithm
- Exam
description: 'My life for pass!'
categories:
- Algorithm
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

# 一般方法

- 动态规划的目标是在所有允许选择的决策序列中选取一个会获得问题最优解的决策序列，即最优决策序列。
- 最优性原理(Principle of Optimality)：过程的最优决策序列具有如下性质：无论过程的初始状态和初始决策是什么，其余的决策都必须相对于初始决策所产生的状态构成一个最优决策序列。
- 动态规划的求解思想可以归纳为两个步骤：

- - 证明问题满足最优性原理
  - 给出递推关系式

# 多段图

## 多段图向前处理递推关系式

设P(i, j)是一条从$$V_i$$中的节点 j 到汇点 t 的最小成本路径, COST(i, j)表示这条路径的成本。根据向前处理方法：
$$
COST(i,j)=\min(c(j,l)+COST(i+1,l))
$$
   其中：$$l∈V_i+1,<j, l>∈E,c(j, l)$$表示该边的成本。

```c
void FGraph (edgeType E, int k, int n, int c[], int P[]) {
    float COST[n];
    int D[n - 1], r;
    COST[n] = 0;
    for(int j = n - 1;j >= 1; j--) { //计算COST(j)
        //设r是一个这样的结点，<i,j>∈E且使c(j,r)+COST[r]取最小值;
        COST[j] = c[j,r] + COST[r];
        D[j]=r;
    };
    //找一条最小成本路径
    P[1] = 1;
    p[k] = n;
    //找路径上的第j个结点
    for(int j = 2;j <= k - 1; j++){
        P[j] = D[P[j - l]];
    };
}
```

* 空间复杂度：除输入参数外，还需要COST、D、P。
* 时间复杂度：$$\Theta(n+e)$$：n是图G中节点个数，e是图G中边的个数。

# 多段图向后处理递推关系式

设BP(i, j)是一条从源点s到$$V_i$$中的结点j的最小成本路径，BCOST(i, j)表示这条路径的成本，根据向后处理方法有：
$$
BCOST(i,j)=\min(c(l,j)+BCOST(i-1,l))
$$
   其中：$$l∈V_i+1,<j, l>∈E,c(l,j)$$表示该边的成本。

# 最优二分检索树

- 一次成功检索在一个内结点(l 级)处终止，算法中的循环要执行l 次，所以内结点$$a_i$$的预期成本为：
  $$
  P(i)×level(a_i)
  $$

- 一次不成功检索在一个外结点处(l 级)终止，算法中循环要执行l−1次，所以外结点$$E_i$$的预期成本为：
  $$
  Q(i)×(level(E_i)−1)
  $$

- 二分检索树的预期成本为：$$ΣP(i)level(a_i)+ΣQ(i)(level(E_i)−1)$$

## 递推公式

$$
C(i, j)=\min_{i<k\le j}{(C(i, k−1)+C(k, j))}+W(i, j)
$$

## 时间复杂度分析

- 按(j−i)=1,2,...,n的顺序去计算C(i, j)。当j−i=m时，有n−m+1个C(i, j)要计算。
- 每个C(i, j)的计算要求找出m个量中的最小值。因此一个这样的C(i, j)能够在Ο(m)时间内计算出来。
- 对于具有j−i=m的所有C(i, j)总的计算时间是Ο(nm−m2)。
- 对于所有的j−i=m，m=1,2,Ö,n，有：$$\sum_{1\le m\le n}(nm−m^2) = Ο(n^3)$$

## D.E.Knuth的优化方法

- 求最优的k可以通过把检索限制在区间R(i,j−1)≤k≤R(i+1,j)求解递归关系式。
- 这种情况下，计算时间为$$O(n^2)$$。

# 0/1背包问题

## 递推公式

$$
f_i(X)=\max{(f_{i−1}(X), f_{i−1}(X−w_i)+p_i)}
$$

## DKP算法

* 支配规则：

  如果Si−1和Si1中一个有(Pj,Wj)，另一个有(Pk,Wk)，并且在Wj≥Wk的同时有Pj≤Pk， 那么，序偶(Pj,Wj)被放弃。称(Pk,Wk)支配(Pj,Wj)。

* 思想：

  1. 初始化$$S_0$$

  2. 由$$S^{i-1}$$生成$$S^i$$（i=1,2,…,n-1）
     * 针对$$S^{i-1}$$中每个j∈[l,u]，生成$$S^i_1$$中序偶(pp,ww),同时进行归并：
       * $$S^{i-1}$$中重量比ww小的 (P(k),W(k))直接加入Si；
       * 检查(pp,ww)是否被支配；
       * 检查Si-1中被(pp,ww)所支配的序偶。
     * $$S^i_1$$中序偶都生成后，$$S^{i-1}$$中若还有序偶没有加入$$S^i$$，则全部加入。

  3. 生成最末序偶，回溯构造最优决策序列。

# 可靠性设计

## 递推公式

$$
f_i(X)=\max{(\phi_{i}(m_i)f_{i−1}(X−c_im_i))}
$$

# 货郎担问题

## 递推公式

$$
g(i,S)=\min{(c_{ij} +g(j,S−\{j\}))},j∈S
$$

## 复杂度分析

* g(i,S)的个数为$$\sum_{0\le k\le n-2}(n−1)C(n−2,k)=(n−1)2^{n−2}$$；
* 求每个g(i,S) 要做|S|−1次比较；
* 总计算时间为$$Θ(n^22^n)$$。

# 流水线调度问题

* 最优调度的排列具有下述性质：在给出了这个排列的第一个作业后，剩下的排列相对于这两台设备在完成第一个作业时所处的状态而言是最优的。

## 调度规则

1. 把全部$$a_i$$和$$b_i$$分类成非降序列；
2. 按照这一分类次序考察此序列：

- - 如果序列中下一个数是aj且作业j还没调度，那么在还没使用的最左位置调度作业j；
  - 如果下个数是bj且作业j还没调度，那么在还没使用的最右位置调度作业j；
  - 如果已经调度了作业j，则转到该序列的下一个数。 