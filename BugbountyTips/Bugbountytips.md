# Bugbountytips

## headers头注入参数

```
1. Host:
2. X-Host:
3. X-Forwarded-For:
4. X-Forwarder- Host:
5. X-Forwarder- Server:
6. Forwarded:
7. X-HTTP-Host- Override:
8. X-Forwarded-Proto headers:
```



## 批量扫描思路

`subfinder -d baidu.com -silent | httpx -silent | nuclei  -t files/ -o results.txt`

`subfinder -d nuomi.com -silent | httpx -silent | nuclei -t nuclei-templates  -o results.txt`

`subfinder -dL baidu.txt -silent | httpx -silent | nuclei -t nuclei-templates  -o results.txt`

## 收集资产

根据favicon图标来搜索:

先获取hash:` iconhash  https://www.zhishidashi.com/favicon.ico`

shodan:

```
http.favicon.hash:-797813492
```

fofa:

```
icon_hash=-797813492
```

其他shodan语法:

```
ssl:target.* 200
http://Ssl.cert.subject.CN:"target.*" 200
```

其他fofa:

```
python3 X-Fofa.py 'cert=" BEIJING JINGDONG SHANGKE INFORMATION TECHNOLOGY CO., LTD."' jd _fofapro_ars_session的值
```



## 测试payload

这个很多骚姿势: [Payloads_xss_sql_bypass](https://github.com/Y000o/Payloads_xss_sql_bypass)

