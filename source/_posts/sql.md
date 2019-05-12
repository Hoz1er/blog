---
title: SQL语句笔记
tags:
    - SQL
    - 随笔
---
# SQL语句

## 获取连续的日期/时间

```sql
declare @Start datetime;
declare @End datetime;
select  [Date] =CONVERT(char(4), DATEADD(YEAR, [number], @Start), 121)
from    master..spt_values
where   type = 'p' and [number] < DATEDIFF(YEAR,@Start,@End) + 1
group by CONVERT(char(4), DATEADD(YEAR, [number], @Start), 121)
```

```sql
--获取连续的整数值
select top 10 [number]
from master..spt_values
where type='p'
order by [number] asc
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

## 生成数据字典

```sql
use your-database  --指明数据库  
go

SELECT  
    表名=case when a.colorder=1 then d.name else '' end,  
    表说明=case when a.colorder=1 then isnull(f.value,'') else '' end,  
    字段序号=a.colorder,  
    字段名=a.name,  
    标识=case when COLUMNPROPERTY(a.id,a.name,'IsIdentity')=1 then '√'else '' end,  
    主键=case when exists(SELECT 1 FROM sysobjects where xtype='PK' and name in (SELECT name FROM sysindexes WHERE indid in(SELECT indid FROM sysindexkeys WHERE id = a.id AND colid=a.colid
   ))) then '√' else '' end,  
    类型=b.name,  
    长度=COLUMNPROPERTY(a.id,a.name,'PRECISION'),  
    占用字节数=a.length,  
    小数位数=isnull(COLUMNPROPERTY(a.id,a.name,'Scale'),0),  
    允许空=case when a.isnullable=1 then '√'else '' end,  
    默认值=isnull(e.text,''),  
    字段说明=isnull(g.[value],'')  
FROM syscolumns a  
    left join systypes b on a.xtype=b.xusertype  
    inner join sysobjects d on a.id=d.id and d.xtype='U' and d.name<>'dtproperties'  
    left join syscomments e on a.cdefault=e.id  
    left join sys.extended_properties g on a.id=g.major_id and a.colid=g.minor_id  
    left join sys.extended_properties f on d.id=f.major_id and f.minor_id =0  
--where d.name='要查询的表' --仅生成指定表的数据字典   
order by d.name,a.id,a.colorder

```