---
title: Excel中将时间戳转换为日期
date: 2018-07-19 11:52:45
categories: 
- 文档
tags: 
- Excel
---
```
=TEXT((B2/1000+8*3600)/86400+70*365+19,"yyyy-mm-dd hh:mm:ss")
```