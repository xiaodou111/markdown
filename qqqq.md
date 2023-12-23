D_mddh_apply   p_mddh_auto_apply

cd_aslk_cgrk  cd_ysckd

ls_sql1[1]="insert into D_mddh_apply(objbusno,wareid,applyqty)values("+ls_busno+","+ls_wareid+","+ls_sumwareqty+")"

d_cgrk_999

230420312051  10305416

查询1-20 -- 4-20 3个月没有来消费

查询4.1 统计1.1-3.31号

d_yb_first_cus  tmp_YB_FIRST_CUS   D_YB_FIRST_CUS_202210

proc_yb_first_md_new1  proc_ybxz_cx  proc_nocome_cus

```
SELECT h.saleno,h.accdate,h.busno,d.wareid,d.saler,d.STDPRICE,d.netprice,d.wareqty 卖出数量,dr.wareqty 调入数量,
row_number() OVER(partition BY h.busno,d.wareid ORDER BY h.accdate ) rn
FROM t_sale_d d
JOIN t_sale_h h   ON h.saleno=d.saleno
JOIN d_zddb_xqsp_dr dr ON dr.objbusno=d.busno AND d.makeno=dr.makeno AND d.wareid=dr.wareid
 WHERE h.accdate>dr.zdate 
```

temp_sqgz_sale proc_xqsale_xg d_sqgz_sale

v_zhybjsjlb  d_zhyb_year_2022:智慧医(3-12月每天数据)+医保结算(1-3月数据)        v_ybjsjlberp d_zhyb_year_2022_temp:医保结算(1-3月数据)

d_zhyb_year_2022_1: (2022年医保顾客排序,取最后消费时间) 

d_zhyb_year_2023(2023年每天的顾客)

D_ZHYB_YEAR_2023_1 (d_zhyb_year_2022_1+d_zhyb_year_2023+取最后消费时间)

d_zhyb_year_2023_zs(2023年来过瑞人堂诊所的)

 d_zhyb_year_2023_yd   proc_ybcust_ydzs

d_zhyb_year_2022_notyd d_zhyb_year_2022_notzs

效期销售奖励跟踪报表2304181525057265

一单重复显示 应完成第一个调拨单后再显示   没卖的也要显示

卖完的备注一下

cd_ydwhf cd_zswhf

D_YB_NEW_CUS_2022 22年新增医保顾客  

D_YB_FIRST_CUS_2022 22年非异地订单

D_YB_NEW_CUS_2023_temp 2023-06-25  临时存放原22年新增客户数据

v_zjys_wl2022_xs 2022年所有客户

```
INSERT INTO D_YB_FIRST_CUS_2022(erpsaleno, 
receiptdate, 
busno, 
saler, 
username, 
customername, 
identityno, 
nb_flag, 
cbd, 
cbdname, 
netsum, 
status, 
yg_flag, 
jslx, 
orderno
)SELECT erpsaleno, receiptdate, a.busno, saler, username, customername, identityno, nb_flag, a.cbd, cbdname, netsum, status, yg_flag,jslx,
a.orderno
  from d_yb_first_cus a 
  left join v_pros_order_receipt_list b    on a.ORDERNO = b.orderno and b.orderstatus not in('0','1','4','7')
  WHERE a.receiptdate<DATE'2023-01-01'
   AND a.CBD IN(331082,331004,331083,331024,331081,331023,331022,331003,331002,331099)
   AND CASE WHEN a.cbd IN ('331099','331002') THEN 331099 ELSE a.cbd END = 
  CASE WHEN substr(b.msgid,2,6) IN ('331099','331002') THEN '331099' ELSE substr(b.msgid,2,6) END 
```

```
SELECT CASE WHEN ts.classcode=30510 THEN '门店' ELSE '诊所' END AS mdlx,CASE WHEN cbdname IN('台州市本级','台州市椒江区') 
THEN '台州市本级' ELSE  cbdname END AS qy from D_YB_NEW_CUS_2023 a
JOIN  t_busno_class_set ts on a.busno=ts.busno and ts.classgroupno ='305' AND ts.classcode IN(30511,30510)
 GROUP BY CASE WHEN cbdname IN('台州市本级','台州市椒江区') 
THEN '台州市本级' ELSE  cbdname END,CASE WHEN ts.classcode=30510 THEN '门店' ELSE '诊所' END
```

```
 --所有诊所的记录
 a3 AS(
 SELECT erpsaleno,receiptdate,a.busno,saler, username, customername,identityno,nb_flag, cbd, cbdname, netsum,status,yg_flag, jslx, orderno
 FROM D_YB_NEW_CUS_2023 a
 JOIN t_busno_class_set ts ON a.busno=ts.busno AND ts.classgroupno='305' AND ts.classcode='30511'
 WHERE a.RECEIPTDATE<DATE'2023-01-01'
 ),
 --所有药店的记录
 a4 AS(
 SELECT erpsaleno,receiptdate,a.busno,saler, username, customername,identityno,nb_flag, cbd, cbdname, netsum,status,yg_flag, jslx, orderno
 FROM D_YB_NEW_CUS_2023 a
 JOIN t_busno_class_set ts ON a.busno=ts.busno AND ts.classgroupno='305' AND ts.classcode='30510'
 WHERE a.RECEIPTDATE<DATE'2023-01-01'
 ),
```

石药销售格式

万彩办公大师

  --生成随机时间 
 for res in (select * from aaaa2 WHERE status=0) loop

IF  to_char(res.month,'YYYY-MM-DD')='2022-02-01' then
  SELECT res.month +
       round(DBMS_RANDOM.VALUE(0,27))+ ROUND(DBMS_RANDOM.VALUE(7.5/24, 20/24), 10)
 into  v_date
FROM dual;
elsif  to_char(res.month,'YYYY-MM-DD') in ('2022-01-01','2022-03-01','2022-05-01','2022-07-01','2022-08-01','2022-10-01','2022-12-01') then
SELECT res.month +
       round(DBMS_RANDOM.VALUE(0,30))+ ROUND(DBMS_RANDOM.VALUE(7.5/24, 20/24), 10)
 into  v_date
 FROM dual;
 ELSE
SELECT res.month +
       round(DBMS_RANDOM.VALUE(0,29))+ ROUND(DBMS_RANDOM.VALUE(7.5/24, 20/24), 10)
 into  v_date
FROM dual;
 END IF; 

  insert into d_baier_otc_suiji
      select v_date_e1 ,res.busno,ceil(res.wareqty*v_xs1),res.wareid from dual
      union all
      select v_date_e2,res.busno,ceil(res.wareqty*v_xs2),res.wareid from dual
      union all
      select v_date_e3,res.busno,ceil(res.wareqty)-ceil(res.wareqty*v_xs1)-ceil(res.wareqty*v_xs2),res.wareid from dual ;

   end loop ;

即时库存和历史库存查询报表一样

cd_aslk_xsck_export  cw_aslk_xsck  商品流向表

cd_aslk_syd_export  cw_aslk_syd   盘点损溢单

cd_aslk_jskc_export   cw_aslk_jskc  即时库存

proc_mdhzhfmx  统计门店患者回访明细

proc_mdhffg

丁佳育报表:零售流水账查询,一个单号有几个收款方式就显示几行,再加实收金额-此付款方式的差额

 罗氏门店视图盘点单

union all
SELECT a.abnormityno,'','浙江瑞人堂医药连锁有限公司',81248,case when wareqtya>wareqtyb then '报溢' else '报损' end ,a.wareid,c.warename,c.warespec,c.wareunit,
a.makeno,wareqtyb-wareqtya,a.SALEPRICE,a.SALEPRICE*abs(wareqtya-wareqtyb),trunc(b.EXECDATE),case when wareqtya>wareqtyb then '报溢' else '报损' end ,'P001' as 库位,
a.invalidate  有效日期,0,'' as syz
  FROM t_abnormity_d  a
inner join t_abnormity_h  b on a.abnormityno=b.abnormityno
left join t_ware_base c on a.wareid=c.wareid
WHERE b.busno=81248 and a.wareid in (SELECT wareid FROM t_ware_luoshi)
and a.wareqtya<>a.wareqtyb  and trunc(b.EXECDATE) >=date'2022-04-01';

11-24门店零售明细客户化窗体

SELECT sb.zmdz1,a.busno,a.memcardno,a.wareqty,a.wareid,b.warename,a.saleno, a.je,a.dj,a.ph,a.finatime,a.invalidate,b.warespec,b.wareunit,c.factoryname,a.paytype
FROM d_aslk_business_sale_md  a 
left join t_ware_base b  on a.wareid=b.wareid
left join t_factory c on b.factoryid=c.factoryid
left join s_busi sb on a.busno=sb.busno

```1539
 --插入券信息
               v_kk2 := 'INSERT INTO T_CASH_COUPON_INFO
                  (COUPON_NO,
                   ISSUING_DATE,
                   COMPID,
                   BUSNOS,
                   COUPON_VALUES,
                   LEAST_SALES,
                   COUPON_TYPE,
                   COUPON_DESC,
                   CARD_NO,
                   MOBILE,
                   START_DATE,
                   END_DATE,
                   USE_STATUS,
                   STATUS,
                   NOTES,
                   CREATEUSER,
                   CREATETIME,
                   GIVE_SALENO,
                   COUPON_KIND,    --1现金券 2折扣券 4特价券(指定价格使用)
                   ADVANCE_PAYAMT, --预约金
                   CREATE_BUSNO,
                   CREATE_TPYE, --发放方式；0促销发券，1手工发券，2线上发券
                   BAK4,
                   BAK6,
                   BAK7,
                   BAK9, --券类型1折扣、2满减、4特价券(指定价格使用)
                   ADV_TYPE )

                  SELECT '||V_COUPONNO||',
                         SYSDATE,
                         '||v_compid||',
                         '||v_busnos||',
                         0 as COUPON_VALUES,
                         0 as LEAST_SALES,
                         ''礼品券'''||p_pst_ware||',
                         ''礼品券'''||p_pst_ware||',
                         '||p_memcardno||',
                         '||V_TEL||',
                         nvl('||pst_begindate||','||user_begindate||'),
                         nvl('||pst_enddate||','||user_enddate||'),
                         0,
                         1,
                         notes,
                         '||p_user||',
                         SYSDATE,
                         '||p_saleno||',
                         2,
                         reserve_amt,
                         '||p_busno||',
                         1 ,
                         wareid,
                         '||V_COUPONNO||',
                         ''礼品券'''||p_pst_ware||'            AS BAK7,
                         3,
                         '||p_reserve_type||'
                    from d_ware_coupon_rsv where compid='||p_compid||' and wareid='||p_wareid||' and reserve_type='||p_reserve_type||'';

                    out_coupon_no := out_coupon_no || ',' || '''' || V_COUPONNO || '''';
                     dbms_output.put_line(v_kk2);
       execute immediate  v_kk2 ;
```

```
sf=select * from
df=delete from
lj=left join
ij=inner join
wh=where
gp=group by
se=select
c*=count(*)
iiv=insert into a values()
iis=insert into a select * from b
us=update set
cor=create or replace
cb=create table
at=alter table tb add name varchar2(40);
atm=alter table tb modify (name nvarchar2(20));
addp=alter table 表名 add constraint 约束名 primary key(字段名);
scs=select compid from s_busi where busno;
zlsp=select a.wareid,b.warename,b.warespec from a join t_ware_base b on a.wareid=b.wareid;
asof=AS OF TIMESTAMP TO_TIMESTAMP('2023-09-09 10:00:00', 'YYYY-MM-DD HH24:MI:SS')

tishi=scs=select compid from s_busi where busno;zlsp=select a.wareid,b.warename,b.warespec from a join t_ware_base b on a.wareid=b.wareid;asof=AS OF TIMESTAMP TO_TIMESTAMP('2023-09-09 10:00:00', 'YYYY-MM-DD HH24:MI:SS') ;iis=insert into a select * from b;us=update set;cor=create or replace;cb=create table;at=alter table tb add name varchar2(40);atm=alter table tb modify (name nvarchar2(20));addp=alter table 表名 add constraint 约束名 primary key(字段名);
```

proc_update_cxtjd
