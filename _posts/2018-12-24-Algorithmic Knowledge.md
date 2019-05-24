---
layout:	post
title:	"Algorithmic Knowledge"
image:	''
date:	2018-12-24 15:09:03
tags:	
- Algorithm
- Exam
description: 'My life for pass!'
categories:
- Algorithm
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

# 算法特性

* **确定性：**每一种运算必须要有确切定义，无二义性。
* **可行性：**运算都是基本运算，原理上能由人用纸和笔在有限时间完成。
* **有穷性：**在执行了有穷步运算后终止。
* **输入：**零个或多个输入。
* **输出：**一个或多个输出。

# 算法学习基本内容

- **设计算法**
- **表示算法**
- **确认算法**
- **分析算法**
- **测试程序**

# 时间复杂度

## 定义

1. 如果存在两个正常数 $$c$$ 和 $$n_0$$ ，对于所有的$$n≥n_0$$，有$$|f(n)|≤c|g(n)|$$，则记做$$f(n)=O(g(n))$$。
2. 如果存在两个正常数 $$c$$ 和 $$n_0$$ ，对于所有的$$n≥n_0$$，有$$|f(n)|≥c|g(n)|$$，则记做$$f(n)=\Omega(g(n))$$。
3. 如果存在两个正常数 $$c_1$$、$$c_2$$和 $$n_0$$ ，对于所有的$$n≥n_0$$，有$$c_1|g(n)|≤|f(n)|≤c_2|g(n)|$$，则记做$$f(n)=\Theta(g(n))$$。

---

* 从计算时间上算法可以分为两类：
  1. 多项式时间算法：$$O(1)<O(\log n)<O(n)<O(n\log n)<O(n^2)<O(n^3)$$
  2. 指数时间算法：$$O(2^n)<O(n!)<O(n^n)$$

---

**Example：**

**Q：**证明：$$O(f(n))+O(g(n))=O(f(n)+g(n))$$。

**A：**
$$
\forall f_1(n) = O(f(n)),\quad \exist c_1,n_1,\quad S.T.\forall n\ge n_1,f_1(n)\le c_1f(n)\\
\forall g_1(n) = O(g(n)),\quad \exist c_2,n_2,\quad S.T.\forall n\ge n_2,g_2(n)\le c_2g(n)\\
令c_3 = \max{\{c1, c2\}},n3 = \max{\{n1, n2\}},h(n) = f(n) + g(n)\\
则\forall n \ge n_3,有：\\
\begin{align*}
O(f(n)) + O(g(n))&=f_1(n) + g_1(n)\\
&\le c_1f(n) + c_2g(n)\\
&\le c_3f(n) + c_3g(n)\\
&= c_3(f(n) + g(n))\\
&=c_3h(n)\\
&=O(f(n) + g(n))
\end{align*}
$$


# 算法问题分类

* **P类问题：**有多项式阶的时间复杂度。
* **顽型问题：**有指数阶的时间复杂度，并被证明无多项式阶的算法。
* **NP问题：**可以在多项式时间内验证一个解。
* **NP-完全问题/NPC问题：**存在这样一个NP问题，所有的NP问题都可以在多项式时间内约化成它。
* **NP-难问题：**不一定是NP问题，所有的NP问题都可以在多项式时间内约化成它。

## 可满足性问题(satisfiability problem)

对于变量的任意一组真值指派确定公式是否为真。若为真则称为可满足的。

## Cook定理

可满足性在**P**内，当且仅当**P**=**NP**。