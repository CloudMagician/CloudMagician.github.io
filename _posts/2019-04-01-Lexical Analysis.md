---
layout:	post
title:	"Lexical Analysis"
image:	''
date:	2019-04-01 20:31:28
tags:	
- Exam
description: 'My life for pass!'
categories:
- Compilation Principle
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

## 词法分析器概述

* 功能
* 单词
* Token

## 正则表达式

* 字母表
* 符号串

## 有限自动机

### 确定有限自动机

### 非确定有限自动机

### DFA化简

### 正则表达式与FA等价

## 词法分析器的设计与实现

### 单词分类

* 标识符
* 常量
* 特殊符号

#### 正则表达式描述

#### 有限自动机描述

### 状态转换

#### 矩阵法

* 特点：程序短小，但占用存储空间多。

#### 图法

* 特点：程序长，但占用存储空间少。

### 特殊处理

#### 保留字识别

* 统一识别：当作一般标识符统一识别；
* 单独识别：分开识别。

#### 符合单词、数

* 复合单词的识别
* 数的转换

#### 控制字符、注释

* 控制字符的处理
* 注释的处理

#### 语义信息存储

* 直接在语义信息部分存储
* 设置标识符表和常量表

### 构造词法分析器步骤

































