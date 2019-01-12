---
layout:	post
title:	"Relational Algebraic Expression"
image:	''
date:	2019-1-8 14:09:35
tags:	
- Database
- Exam
description: 'My life for pass!'
categories:
- Database
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

## 码

### superkey

超码，是一个或多个属性的集合，这些属性的组合可以使我们在一个关系中唯一的标识一个元祖。

### candidate key

候选码，指任意真子集都不能成为超码的最小超码。

### primary key

主码，代表被数据库设计者选中的、主要用来在一个关系中区分不同元祖的候选码。

### foreign key

外码，一个关系模式（如r1）可能在它的属性中包括另一个关系模式（如r2）的主码，这个属性在r1上称作参照r2的外码。

## 关系运算

### 基本运算

* 选择运算：$$\sigma$$
* 投影运算：$$\Pi$$
* 并运算：$$\bigcup$$
* 差运算：$$-​$$
* 笛卡尔积：$$\times​$$
* 更名运算：$$\rho$$

### 附加的关系代数运算

* 交运算：$$\bigcap$$（$$r\cap s=r-(r-s)$$）
* 自然连接：$$\bowtie​$$

