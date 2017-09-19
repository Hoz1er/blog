---
title: SQL语句
tags:
- SQL
---
  
## 获取连续的日期/时间  
```sql
declare @Start datetime;
declare @End datetime;
select  [Date] =CONVERT(char(4), DATEADD(YEAR, [number], @Start), 121)  
from    master..spt_values  
where   type = 'p' and [number] < DATEDIFF(YEAR,@Start,@End) + 1  
group by CONVERT(char(4), DATEADD(YEAR, [number], @Start), 121

```
>注：最多2048行记录  

## 向自增列插入指定数值  
```sql
--表t 字段： [ID]:自增主键 [Name]
SET IDENTITY_INSERT [dbo].[t] ON
insert into [dbo].[t] ([Id],[Name]) values(1,'1')
SET IDENTITY_INSERT [dbo].[t] OFF
```
>注：  
>1.请及时使用`SET IDENTITY_INSERT [dbo].[t] OFF`关闭自增列的自定义数值插入  
>2.每一次连接会话中的任一时刻，只能对一个表设置IDENTITY_INSERT ON，且设置只对当前会话有效  
>3.插入时一定要列出该标识列  

<!-- More -->

