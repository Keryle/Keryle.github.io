---
layout: default
title: Timer
description: 定时器的使用
---
#### TMOD

| 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| - | - | - | - | - | - | - | - |
|GATE | C/T | M1 | M0 | GATE | C/T | M1 | M0 |

* 不可位寻址，复位值：00H，高四位控制T1，低四位控制T0
* GATE：门控位，当GATE=1，需要INT0/INT1引脚置1才可开启定时计数器
* C/T：置1为计数器，置0为计数器
*

[back](./)
