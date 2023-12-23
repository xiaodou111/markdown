##### sql优化

**查询条件不能用 <> 或者 !=**

```
使用索引列作为条件进行查询时，需要避免使用<>或者!=等判断条件。

如确实业务需要，使用到不等于符号，需要在重新评估索引建立，避免在此字段上建立索引，改由查询条件中其他索引字段代替。
```

**隐式类型转换造成不使用索引**

如下 SQL 语句由于索引对列类型为 varchar，但给定的值为数值，涉及隐式类型转换，造成不能正确走索引。

```
select col1 from table where col_varchar=123;  
```

**1、避免在索引列上使用NOT ，** 我们要避免在索引列上使用NOT, NOT会产生在和在索引列上使用函数相同的影响. 当ORACLE”遇到”NOT,他就会停止使用索引转而执行全表扫描.

**2、避免在索引列上使用计算．** WHERE子句中，如果索引列是函数的一部分．优化器将不使用索引而使用全表扫描． 举例:

```
低效:SELECT … FROM DEPT WHERE SAL * 12 > 25000;
高效:SELECT … FROM DEPT WHERE SAL > 25000/12;
```

**3、避免在索引列上使用IS NULL和IS NOT NULL**

```
低效:(索引失效) SELECT … FROM DEPARTMENT WHERE DEPT_CODE IS NOT NULL;
高效:(索引有效) SELECT … FROM DEPARTMENT WHERE DEPT_CODE >=0;
```

**4、注意通配符%的影响** 使用通配符的情况下Oracle可能会停用该索引。如 :

```
SELECT…FROM DEPARTMENT WHERE DEPT_CODE like ‘%123456%'（无效）。
SELECT…FROM DEPARTMENT WHERE DEPT_CODE = ‘123456'（有效）
```
