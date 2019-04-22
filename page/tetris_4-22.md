---
layout: default
title: 俄罗斯方块
description: 51单片机编写
---
# 演示视频

<iframe width="560" height="315" src="https://showmore.com/zh/embed/xh4jdpm"  frameborder="0" allow="encrypted-media" allowfullscreen></iframe>

# 数据结构

![](picture/方块.PNG)

一个俄罗斯方块用一个int类型（两字节）表示。
游戏数据用一个int数组保存。

# 算法

## 方块移动

以下移操作为例：
![](picture/操作算法.PNG)

C 中保存了需要改变的像素点信息，再与A 进行比较，判断出是变暗，还是点亮。
