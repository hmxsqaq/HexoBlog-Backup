---
title: 【游戏编程模式】读书笔记六：序列模式（更新中）
tags:
  - 《游戏编程模式》
  - 设计模式
  - Note
categories:
  - Notes
  - Design Pattern
  - 《Game Programming Patterns》
description:
  - 序列模式
cover: 'https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202303301905867.jpeg'
abbrlink: 2006
date: 2023-04-09 00:00:00
---

![img](https://hmxs-1315810738.cos.ap-shanghai.myqcloud.com/img/202304070014396.jpeg)

> **《游戏编程模式》-Bob Nystrom**
>
> **书籍汉化版网址：https://gpp.tkchu.me/**
>
> **笔记目录：[https://hmxs.games/posts/2000/](https://hmxs.games/posts/2000/)**

---

# 序列模式

在游戏制作中，“时间”是一个极其重要的概念，我们需要为我们创造的虚拟世界赋予时钟。本章节序列模式(SequencingPatterns)便是构建时钟的工具。

**游戏循环**是时钟的主轴

对象通过**Update方法**响应游戏循环

应用**双缓冲模式**可以储存快照来隐藏计算机顺序执行的特性，使游戏看起来是同步更新的

---

## 双缓冲模式