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
9.X-HTTP-Method -Override: PUT
```

### 任意密码重置

发送重置密码邮件的时候可以增加:

X-Forwarded-Host: xss.sb





### 绕过401

```
X-Custom-IP-Authorization: 127.0.0.1
X-Forwarded-Origin: 127.0.0.1
```



## 批量扫描思路

`subfinder -d baidu.com -silent | httpx -silent | nuclei  -t files/ -o results.txt`

`subfinder -d nuomi.com -silent | httpx -silent | nuclei -t nuclei-templates  -o results.txt`

`subfinder -dL baidu.txt -silent | httpx -silent | nuclei -t nuclei-templates  -o results.txt`



获取json文件

```
1.assetfinder http://tesla.com | waybackurls | grep -E "\.json(?:onp?)?$" | anew
2.assetfinder $(cat domain.txt) |anew| waybackurls | grep -E "\.json(?:onp?)?$" |anew
3. assetfinder $(cat domain.txt) |anew| httpx -silent -mc 200,301 --threads 100|waybackurls | grep -E "\.json(?:onp?)?$" |anew
4.cat domain.txt | assetfinder -subs-only | anew | httpx -silent -mc 200,301 | waybackurls
5.cat domain.txt| assetfinder -subs-only|anew |waybackurls |grep -E "\.json(?:onp?)?$"|anew|httpx --title --mc 200,301,302 -o youzhanok.txt
6. cat target.txt | assetfinder -subs-only |anew | waybackurls| anew| grep -v 'jpg' |  httpx -silent -mc 200,301 -o resultAll.txt
7.cat target.txt | assetfinder -subs-only |grep -v 'static\|img' |anew | waybackurls| anew| grep -v 'jpg' |  httpx -silent -mc 200,301 -o resultAll.txt
```



批量扫描js文件

JSFSCAN.h

>```
>docker run -v $(pwd):/jsfscan -it jsfscan  "/bin/bash"
>```

```
1.sed   "s/^/http:\/\//g"  targets.txt > targets1.txt
2.bash JSFScan.sh -l target.txt  --all -r -o target.ru
```





批量扫描账户接管

- git clone https://github.com/r3curs1v3-pr0xy/sub404.git
- Install dependencies: pip install -r requirements.txt
- Install [Subfinder](https://github.com/projectdiscovery/subfinder) (optional)
- Install [Sublist3r](https://github.com/aboul3la/Sublist3r) (optional)
- python3 sub404.py -h

example:

```
 sub404 -d qq.com
```



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



利用第三方服务提取:

(1)

```
curl --silent https://sonar.omnisint.io/subdomains/didichuxing.com | grep -oE "[a-zA-Z0-9._-]+\.didichuxing.com" | sort -u
```

(2)

```
curl -s "https://jldc.me/anubis/subdomains/sony.com" | grep -Po "((http|https):\/\/)?(([\w.-]*)\.([\w]*)\.([A-z]))\w+" | httpx -silent -threads 300 | anew
```









## 测试payload

这个很多骚姿势: [Payloads_xss_sql_bypass](https://github.com/Y000o/Payloads_xss_sql_bypass)

