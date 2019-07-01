---
layout:	post
title:	"Runtime Storage Space Management"
image:	''
date:	2019-05-06 08:02:11
tags:	
- Exam
description: 'My life for pass!'
categories:
- Compilation Principle
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

# 存储方式与存储策略

## 运行时的存储空间结构

- 要保存的信息：目标代码；数据；库函数代码；过程活动的控制信息等
- 运行时的存储空间结构：堆栈-栈区——静态区——目标代码——库代码

## 目标程序运行时的活动

- 活动：过程的一次执行。如果a和b是两个活动，则它们的生存期或者是不重叠的，或者是嵌套的。
- 活动树：由活动构成的一个树，其中

1. 1. 树中的每个节点代表一个活动。	
   2. 树的根节点是程序的主过程（函数）的活动。
   3. 在树中若b为a的儿子节点，则必有a活动调用了b活动。
   4. 在树中若a为b的左兄弟节点，则必有a活动先于b活动执行。

## 名字的作用域和绑定

- 作用域：一个声明起作用的程序部分称为该声明的作用域。一个名字在程序中只声明一次，该名字在程序运行时也可能代表不同的数据对象。 
- 环境和状态：环境表示将名字映射到存储单元的函数，状态表示将存储单元映射到它所保存的值的函数 。
- 绑定：如果环境将名字x映射到存储单元s，我们就说x被绑定（binding）到s。

# 过程活动记录

- 过程活动记录(AR)：过程的一个现场记录

- 记录内容：

- - 过程控制信息：先行活动记录的动态链指针、返回地址、返回值、层数和长度等
  - 机器状态信息：寄存器状态等
  - 全局变量信息:非局部变量的信息
  - 局部变量值：形参变量、局部变量和临时变量

## 抽象地址分配

I.	（ℓ，off）→ LabelDec →（ℓ，off）//程序结构      

​	（ℓ，off）→ ConstDec →（ℓ，off）

​	（ℓ，off）→ TypeDec →（ℓ，off）       

​	（ℓ，off）→ VarDec→（ℓ，off+n）

​	（ℓ，off）→ ProcDec →（ℓ，off）       

​	（ℓ，off）→ FuncDec →（ℓ，off）

II. (ℓ，off）→ T * ID →（ℓ，off+1）//类型声明

​	  (ℓ，off）→ T ID →（ℓ，off+n）

III. (ℓ，off）→ Proc p( →（ℓ+1，off0）//函数声明

​	  (ℓ，off）→ Func f( →（ℓ+1，off0）

​	  (ℓ，off）→ Proc P( →（ℓ+1，off0）

​	  (ℓ，off）→ Fucn F( →（ℓ+1，off0）

IV. (ℓ，off）→ Proc p() →（ℓ+1，off1+ℓ+1）//函数声明

​	 (ℓ，off）→ Func f() →（ℓ+1，off1+ℓ+1）

​	 (ℓ，off）→ Proc P() →（ℓ，off+1）

​	 (ℓ，off）→ Func F() →（ℓ，off+1） 

## 静态存储分配

- 存储对象的存储位置在程序的整个生命周期是固定的。 
- 分配对象：全程变量  常量   信息表
- 分配方法：块地址法：(DataArea,Offset)、 变址模式：(Register,Offset)

## 堆区的存储分配

- 可随时分配和释放空间

- 存储对象：动态申请空间的变量的值

- 释放空间方法：

- - 显式释放：
  - 隐式释放：单指针释放、计数释放法、标记释放法

- 分配空间方法：最佳符合法；首次优先法；循环首次优先法

## 栈式存储分配

- 存在递归调用

- 存储对象：过程中被声明的形参、局部变量  临时变量

- 分配方法：对每个被调用过程分配一段存储空间，sp存放当前过程空间的开始地址；对变量X：（Level，off),则其存放地址为off[sp]。过程结束时自动释放空间；

- 不能存储：

- - 值的生命周期长于过程的变量；
  - 动态申请空间的变量；

# 运行时变量访问的实现

## 辅助定义

### 过程层数

假定主程序的层数为0，称为第0层过程。如果过程   Q是在层数为i的过程P内定义，并且P是包围Q的最小过程，那么，Q的层数就为i+1。

### 调用链：过程名序列

若M是主程序名，则（M）是一个调用链；

若(M,**…**,R)是调用链，且在R中有S的调用，则(M, **…**, R, S)也是调用链。记为CallChain(S)= (M, **…**,R, S)

### 动态链：过程活动记录序列

如果有调用链CallChain(S)=(M,**…**,R, S)，则它对应的动态链为：DynamicChain=[AR(M),**…**,AR(R),AR(S)]

### 声明链：过程名序列

（M）是过程声明链；若（M，**…**，P）是声明链，且P中有过程Q的声明，则（M，**…**，P，Q）也是过程声明链。记作DeclaChain（Q）= （M，**…**，P，Q）

### 活跃活动记录

过程S在动态链中可有多个AR，但只有最新的AR(S)是可访问的，称其为S活跃活动记录，记为LAR(S)。

### 变量访问环境

Q的声明链中的每个过程的活跃活动记录构成的链称为Q的当前变量访问环境，记为：VarVisitEnv(LAR(Q))=[LAR(M),…, LAR(P), LAR(Q)]

## 变量访问环境的实现方法

### 局部Display表方法

- 对于每个AR求出其变量访问环境，并把它以地址表的形式(Display表）保存在AR中。因为每个AR都自带Display表，称这种方法为局部化Display表方法。
- 如果层数为N的过程P的变量访问环境为：VarVisitEnv(AR(P))=[ARi1,**…**,ARi,n+1],arij表示ARij的始地址，则[ari1,**…**,ari,n+1]是AR(P)的Display表.   

#### 局部Display表时变量的访问

对于变量X（L,off）,其中off表示X从数据区开始的偏移，D表示由AR起始地址到数据区的偏移，则X的地址计算如下：

- 当L= CurrentAR.level时：Addr(X)=[sp]+D+off
- 否则:addr(X)=CurrentAR.Display[L]+D+off即[sp+L]+D+off

### 静态链方法

原Display表部分变成一个单元，称为静态链单元，存放静态链指针。

静态链指针的确定：若NewAR.level= CurrentAR.level+1-k，则  NewAR.StaticChainPointer=Indir(sp,k)其中Indir(sp,k)表示sp的k次间接内容

#### 使用静态链时变量的访问

对于变量X（L,off）,其中off表示X从数据区开始的偏移，D表示由AR起始地址到数据区的偏移，则X的地址计算如下：

- 若L= CurrentAR.Level,则addr(X)= [sp]+D+off  
- 若L= CurrentAR.Level-k,则addr(X)= Indir(sp,k)+D+off

### 全局Display表

设置一个总的Display表，其长度为最大嵌套层数（系统确定），其中Display[i]存放第i层最新AR的指针。

- 当层数为j的过程Q被调用时：将旧的Display[j]的内容保存到NewAR(Q)中：NewAR(Q).RessumeAddr = Display[j]改写Display[j]为NewAR(Q)的地址；
- 当退出Q时：恢复原来Display[j]的内容：Display[j]=CurrentAR.ResumeAddr变量X(L,off)的地址：Display[L]+D+off