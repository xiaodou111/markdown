###### 用来看更新了多少行

```
rows_updated NUMBER;
rows_updated := SQL%ROWCOUNT;
DBMS_OUTPUT.PUT_LINE('Rows updated: ' || rows_updated);
```

设置作业

```
begin
  sys.dbms_scheduler.create_job(job_name            => 'H2.每天更新新增医保农保新客明细',
                                job_type            => 'STORED_PROCEDURE',
                                job_action          => 'proc_yb_first_md_new1',
                                start_date          => to_date('23-04-2023 00:33:00', 'dd-mm-yyyy hh24:mi:ss'),
                                repeat_interval     => 'Freq=DAILY;Interval=1',
                                end_date            => to_date('31-12-9999 00:00:00', 'dd-mm-yyyy hh24:mi:ss'),
                                job_class           => 'DEFAULT_JOB_CLASS',
                                enabled             => true,
                                auto_drop           => false,
                                comments            => '每天更新新增医保农保新客明细');
end;
/
```

##### 删除主键重复数据保留一条

```
DELETE FROM t_sale_mes_customer_temp2
  WHERE ROWID NOT IN (
    SELECT MAX(ROWID)
    FROM t_sale_mes_customer_temp2
    GROUP BY saleno
  );
```

```
ROW_NUMBER() OVER (PARTITION BY IDENTITYNO ORDER BY receiptdate)
```

作业关闭/开启

```
BEGIN DBMS_SCHEDULER.DISABLE('每天更新效期销售奖励跟踪'); END;
BEGIN DBMS_SCHEDULER.ENABLE('每天更新效期销售奖励跟踪'); END; 
```

no data 处理,一般用在select into

```
BEGIN
  SELECT MSG 
   INTO v_MSG_rap
   FROM hd_msg_out_bak where MSG_ID =v_MSG_ID_rap
   UNION ALL
   SELECT MSG FROM hd_msg_out_error where  MSG_ID =v_MSG_ID_rap ; 
   DBMS_OUTPUT.PUT_LINE('退仓申请单号:' ||v_distno || v_MSG_rap);
   EXCEPTION
    WHEN NO_DATA_FOUND THEN
      DBMS_OUTPUT.PUT_LINE('这不是一个退仓申请单号');

  END;
```

###### 

随机生成订单

```
SELECT to_char(zdate,'yymmdd')||FLOOR(DBMS_RANDOM.VALUE(100000, 1000000)) FROM d_baier_sale
 END;
```

##### select into没有找到数据

```
BEGIN
EXCEPTION 
 WHEN NO_DATA_FOUND THEN 
       DBMS_OUTPUT.PUT_LINE('数据库中没有编码为'||v_empno||'的员工'); 
 WHEN TOO_MANY_ROWS THEN 
       DBMS_OUTPUT.PUT_LINE('程序运行错误!请使用游标'); 
 WHEN OTHERS THEN 
       DBMS_OUTPUT.PUT_LINE(SQLCODE||'---'||SQLERRM); 
 END;
```

###### 定义触发器，使用RAISE_APPLICATION_ERROR阻止没有员工姓名的新员式记录插入：

```
CREATE OR REPLACE TRIGGER tr_insert_emp 
 BEFORE INSERT ON employees 
 FOR EACH ROW 
 BEGIN 
 IF :new.first_name IS NULL OR :new.last_name is null THEN 
     RAISE_APPLICATION_ERROR(-20000,'Employee must have a name.'); 
 END IF; 
 END;
```

依据视图创建表结构

```
CREATE TABLE my_table AS
SELECT * FROM my_view WHERE 1=0;
```

行列转换

```
SELECT b.wareid,
SUM(DECODE(l.dddwlistdisplay,'美团',round(b.wareqty,6),0))  AS 美团销售额,
SUM(CASE WHEN l.dddwlistdisplay='美团' THEN round(b.wareqty,6) ELSE 0 END ) AS 美团销售额,
SUM(CASE WHEN l.dddwlistdisplay='饿了么' THEN round(b.wareqty,6) ELSE 0 END ) AS 饿了么销售额,
SUM(CASE WHEN l.dddwlistdisplay='京东到家' THEN round(b.wareqty,6) ELSE 0 END ) AS 京东到家销售额,
SUM(CASE WHEN l.dddwlistdisplay='京东健康' THEN round(b.wareqty,6) ELSE 0 END ) AS 京东健康销售额,
SUM(CASE WHEN l.dddwlistdisplay IN('美团','饿了么','京东到家','京东健康') THEN round(b.wareqty,6) ELSE 0 END ) as saleqty 
from  d_rpt_sale_o2o b
JOIN s_dddw_list l 
on l.dddwname = '222' and l.dddwlistdata = b.paytype
WHERE b.accdate BETWEEN TRUNC(SYSDATE)-30 AND TRUNC(SYSDATE)
and EXISTS(SELECT 1 FROM t_busno_class_set wc__ 
 WHERE wc__.busno = b.busno AND wc__.classgroupno = '320' AND wc__.classcode <> '320105' )
GROUP BY b.wareid
```

```
V_SALE_DDI_81499
中的 substr(a.notes,0,decode(instr(a.notes,' '),0,length(a.notes)+1,instr(a.notes,' '))-1)
截取第一个字符到空格相当于REGEXP_SUBSTR(a.notes, '^[^ ]+'),效率可能更高,先不换
```

查询原先创建视图的语句

```
select view_name,text from (select * From USER_VIEWS 
AS OF TIMESTAMP TO_TIMESTAMP('2023-11-16 10:00:00', 
'YYYY-MM-DD HH24:MI:SS') 
)
WHERE REGEXP_LIKE(VIEW_NAME, 'aslk', 'i');
--DBA_VIEWS
```

```
ALL_VIEWS：ALL_VIEWS 视图显示了当前用户有权访问的所有视图的信息，包括视图所有者、视图名称、视图的定义等。

DBA_VIEWS：DBA_VIEWS 视图显示了数据库中所有视图的信息，包括视图的所有者、名称、定义等。要查询 DBA_VIEWS 视图，需要具有 DBA 权限。

USER_VIEWS：USER_VIEWS 视图显示了当前用户拥有的视图的信息，包括视图的名称、定义等。与 ALL_VIEWS 类似，但 USER_VIEWS 只显示当前用户拥有的视图，无需特殊权限即可查询。

ALL_OBJECTS：ALL_OBJECTS 视图提供了数据库中所有对象的信息，包括表、视图、过程、函数等。您可以使用 OBJECT_TYPE 列来过滤出特定类型的对象。

ALL_TAB_COLUMNS：ALL_TAB_COLUMNS 视图包含了所有表和视图的列信息，包括列名、数据类型、约束等。通过过滤 TABLE_NAME 和 VIEW_NAME 列，可以检索特定表或视图的列信息。
```

查看存储过程text

```
SELECT * from ALL_SOURCE WHERE  TYPE='PROCEDURE' AND REGEXP_LIKE(NAME, 'p_dir_auto_zx', 'i');
```

查看哪些存储过程使用了't_remote_prescription这个表

```
SELECT OWNER, NAME, TYPE
FROM DBA_SOURCE
WHERE REGEXP_LIKE(TEXT, 't_remote_prescription_h', 'i');
```

1.列转行 - 使用 `UNPIVOT`：

2.行转列 - 使用 `PIVOT`：

更改作业时间

```
BEGIN
  DBMS_SCHEDULER.SET_ATTRIBUTE(
    name      => 'H2.每天更新新增医保农保新客明细',
    attribute => 'repeat_interval',
    value     => 'FREQ=DAILY;BYHOUR=14;BYMINUTE=45');
END;
```

时间戳

```
ceil((to_date(to_char(时间字段,'yyyy-mm-ddhh24:mi:ss'),'yyyy-mm-ddhh24:mi:ss')
-to_date('1970-01-01 08:00:00','yyyy-mm-dd hh24:mi:ss'))*86400) as createtime
```

去掉空格

```
UPDATE d_o2o_ware_tm
SET JDJK = RTRIM(REGEXP_REPLACE(JDJK, '(.*?)( +)$', '\1'))
WHERE JDJK IS NOT NULL;
```

时分秒

```
UPDATE t_card_addmoney
SET CREATETIME =to_date('2023-09-30 14:00:00', 'yyyy-mm-dd hh24:mi:ss'),
LASTTIME = to_date('2023-09-30 14:00:00', 'yyyy-mm-dd hh24:mi:ss')
 where cardno='16871402327797953' and billno='2310071000001'
```

抛出错误

```
RAISE_APPLICATION_ERROR(-20001, '单轨制更新(处方来源单号)没有行受到影响');
```
