---
title: Mysql 数据类型.md
date: 2018-03-03 11:15:12
categories: server
tags: [db] 
---
MySQL中定义数据字段的类型对你数据库的优化是非常重要的。

MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。

<!--more-->
### 数值类型
MySQL支持所有标准SQL数值数据类型。

这些类型包括严格数值数据类型`(INTEGER、SMALLINT、DECIMAL和NUMERIC)`，以及近似数值数据类型`(FLOAT、REAL和DOUBLE PRECISION)`。

关键字INT是INTEGER的同义词，关键字DEC是DECIMAL的同义词。

BIT数据类型保存位字段值，并且支持MyISAM、MEMORY、InnoDB和BDB表。

作为SQL标准的扩展，MySQL也支持整数类型TINYINT、MEDIUMINT和BIGINT。下面的表显示了需要的每个整数类型的存储和范围。

![logo](/images/server/数据库/mysqlint.png) 

备注: 
1.BIT[M]

位字段类型，M表示每个值的位数，范围从1到64，如果M被忽略，默认为1

2.TINYINT[(M)] [UNSIGNED] [ZEROFILL]  M默认为4,占1个字节

很小的整数。带符号的范围是-128到127。无符号的范围是0到255。

3. BOOL，BOOLEAN

是TINYINT(1)的同义词。zero值被视为假。非zero值视为真。

4.SMALLINT[(M)] [UNSIGNED] [ZEROFILL] M默认为6,占2个字节

小的整数。带符号的范围是-32768到32767。无符号的范围是0到65535。

5.MEDIUMINT[(M)] [UNSIGNED] [ZEROFILL] M默认为9,占3个字节

中等大小的整数。带符号的范围是-8388608到8388607。无符号的范围是0到16777215。

6. INT[(M)] [UNSIGNED] [ZEROFILL]   M默认为11,占4个字节

普通大小的整数。带符号的范围是-2147483648到2147483647。无符号的范围是0到4294967295。

7.BIGINT[(M)] [UNSIGNED] [ZEROFILL] M默认为20,占8个字节

大整数。带符号的范围是-9223372036854775808到9223372036854775807。无符号的范围是0到18446744073709551615。

注意：这里的M代表的并不是存储在数据库中的具体的长度，以前总是会误以为int(3)只能存储3个长度的数字，int(11)就会存储11个长度的数字，这是大错特错的。

tinyint(1) 和 tinyint(4) 中的1和4并不表示存储长度，只有字段指定zerofill是有用，
如tinyint(4)，如果实际值是2，如果列指定了zerofill，查询结果就是0002，左边用0来填充。

### 日期和时间类型

表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。

TIMESTAMP类型有专有的自动更新特性

![logo](/images/server/数据库/mysqldate.png) 

### 字符串类型

字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型

![logo](/images/server/数据库/mysqlchar.png) 

