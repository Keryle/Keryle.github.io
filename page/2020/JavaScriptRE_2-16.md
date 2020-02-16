---
layout: default
title: JavaScript正则表达式
description: 笔记
---

# 什么是正则表达式

正则表达式是由一个字符序列形成的搜索模式。  
在文本中搜索数据时，可以用搜索模式来描述查询内容。  
正则表达式可用于所有文本搜索和文本替换操作。

# 语法

> /正则表达式主体/修饰符(可选)

# 使用字符串方法

JavaScript字符串的两个方法：search() 和 replace()。  
search()方法返回字符串起始位置。
replace()方法在字符串中用一些字符替换另一些字符。

## search()方法示例

> var str = "Visit Runoob!";
> var n = str.search(/Runoob/i);

输出结果为：6， 首位为0；

## replace()方法示例

> var str = document.getElementById("demo").innerHTML;
> var txt = str.replace(/microsoft/i,"Runoob");

输出结果为：Visit Runoob!

# 正则表达式修饰符

|修饰符|描述|
|-|-|
|i|执行对大小写不敏感的匹配。|
|g|执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。|
|m|执行多行匹配。|

# 正则表达式模式

方括号用于查找某个范围内的字符：  
|表达式|描述|
|-|-|
|[abc]|查找方括号之间的的任何字符|
|[0-9]|查找任何从 0 至 9 的数字。|
|(x|y)|查找任何以 | 分隔的选项。|

元字符是拥有特殊含义的字符：  
|元字符|描述|
|-|-|
|\d|查找数字|
|\s|查找空白字符|
|\b|匹配单词边界|
|\uxxxx|查找以十六进制数 xxxx 规定的 Unicode 字符。|

元字符是拥有特殊含义的字符：  
|元字符|描述|
|-|-|
|\d|查找数字|
|\s|查找空白字符|
|\b|匹配单词边界|
|\uxxxx|查找以十六进制数 xxxx 规定的 Unicode 字符。|
