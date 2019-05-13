---
title: sql基本优化
tags:
    - sql
    - 随笔
---

#### 对查询进行优化，要尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引
    
#### 会导致引擎放弃使用索引而进行全表扫描的操作及部分优化思路
* 对字段进行null值判断：  
    尽可能使用NOT NULL 填充数据库  
* != 或 <>  
* where子句中使用 'or' 来连接条件时，某子条件中存在无索引的字段：
    ```sql
    select id from t where col1=1 or col2='123'
    --可修改为：
    select id from t where col1=1
    union
    select id from t where col2='123'
    ```
* in / not in ：  
    * 对连续的数值可使用 between 代替
    * 用 exists 代替
        ```sql
        select id from t1 
        where col1 in (select col1 from t2)
        --可修改为：
        select id from t1
        where exists(select 1 from t2 where t1.col1 = t2.col1)
        ```
* 通配符'%'：  
    * '%123' 无法使用正常索引,可使用反向索引
    * '123%' 可使用正常索引
    * '%123%' 无法使用索引:  
        可以考虑使用全文检索
* where 子句中对字段进行表达式操作
* where 子句中在'='左边进行函数、算术运算或其他表达式运算
* 设置了复合索引，但where子句中未使用到该索引的第一个字段作为条件：  
    需要使用复合索引的第一个字段，且尽量保持字段顺序与复合索引中的字段顺序一致  

#### Update语句尽量只Update需要的字段，否则频繁调用会引起明显的性能消耗，同时带来大量日志

