---
title: MySQL禁止外键检查
date: 2020-09-27 09:22:40
tags: MySQL
---

在有外键约束的MySQL中进行数据导入或者更新时，容易碰到外键约束导致数据无法导入，可以执行下面的语句关闭外键检查：
``` sql
SET FOREIGN_KEY_CHECKS=0;
```
操作完成后应该再开启：
``` sql
SET FOREIGN_KEY_CHECKS=1;
```
[参考](https://stackoverflow.com/questions/15501673/how-can-i-temporarily-disable-a-foreign-key-constraint-in-mysql)