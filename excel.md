ctrl+shift+↓ 选择区域到最后一行

插入:数据透视表

```
INSERT INTO d_jc_busno(wareid, busno, orgname, month,sl)VALUES('10100765','84029','金华瑞人堂保济堂医药连锁有限公司黄杨梅店','44562','1');
```

="insert into d_baier_business_lsbb(ACCDATE,saleno,busno) values("&C2&",'"&B2&"','"&D2&"')"

excel去重

```
数据栏右侧点击 删除重复值 选择重复字段
```

多字段查重

```
=A2&B2&C2&D2 主键合并并填充   点击开始-条件格式-突出显示单元格规则-重复值
```

加一列行号(id)

```
=ROW()-1  用公式前文本框设置为常规
```

 先选一个单元格 然后往下选中区域按shift 再按ctrl+D填充
