---
layout:	post
title:	"Relational Model"
image:	''
date:	2019-1-10 17:09:35
tags:	
- Database
- Exam
description: 'My life for pass!'
categories:
- Database
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

## 实体-联系图

### 基本结构

* 分成两部分的矩形代表实体集。第一部分名字，第二部分属性。
* 菱形代表联系集。
* 未分割的矩形代表联系集的属性。构成主码的属性以下划线表明。
* 线段将实体集连接到联系集。
* 虚线将联系集属性连接到联系集。
* 双线显示实体在联系集中的参与度。
* 双菱形代表连接到弱实体集的标志性联系集。

### 映射基数

* 箭头表明单个。

### 弱实体集

* 弱实体集：没有足够属性生成主码。存在依赖于标志实体集。
* 弱实体集的分辨符以虚下划线表明，而不是实线。
* 关联弱实体集和标志性强实体集的联系集以双菱形表示。

