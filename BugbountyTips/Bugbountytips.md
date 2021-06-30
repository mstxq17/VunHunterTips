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

![image-20210428000911871](Bugbountytips.assets/image-20210428000911871.png)

发送重置密码邮件的时候可以增加:

X-Forwarded-Host: xss.sb



## header头Bypass

这里Chrome可以使用ModHeader插件,然后配置

```
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1   
X-Forwarded-Host : 127.0.0.1 
X-Client-IP : 127.0.0.1 
X-Host : 127.0.0.1 
Forwarded: 127.0.0.1
X-Forwarded-By: 127.0.0.1
X-Forwarded-For-IP: 127.0.0.1
X-True-IP: 127.0.0.1
```

![image-20210517170452356](Bugbountytips.assets/image-20210517170452356.png)

### 绕过401

```
X-Custom-IP-Authorization: 127.0.0.1
X-Forwarded-Origin: 127.0.0.1
```



## 批量扫描思路

key分享的带端口的一种扫描:

```
cat domains.txt | ksubdomain -f 312W_Subdomains.txt -silent -o subdomains_res.txt | naabu -p 80,443,7000-7099,8080-8089 -silent -o naabu_res.txt | httpx -silent | nuclei -t nuclei-templates/ -o nuclei_res.txt
```



`subfinder -d baidu.com -silent | httpx -silent | nuclei  -t files/ -o results.txt`

`subfinder -d nuomi.com -silent | httpx -silent | nuclei -t nuclei-templates  -o results.txt`

`subfinder -dL baidu.txt -silent | httpx -silent | nuclei -t nuclei-templates  -o results.txt`



获取json文件

(1)

```
1.assetfinder http://tesla.com | waybackurls | grep -E "\.json(?:onp?)?$" | anew
2.assetfinder $(cat domain.txt) |anew| waybackurls | grep -E "\.json(?:onp?)?$" |anew
3. assetfinder $(cat domain.txt) |anew| httpx -silent -mc 200,301 --threads 100|waybackurls | grep -E "\.json(?:onp?)?$" |anew
4.cat domain.txt | assetfinder -subs-only | anew | httpx -silent -mc 200,301 | waybackurls
5.cat domain.txt| assetfinder -subs-only|anew |waybackurls |grep -E "\.json(?:onp?)?$"|anew|httpx --title --mc 200,301,302 -o youzhanok.txt
6. cat target.txt | assetfinder -subs-only |anew | waybackurls| anew| grep -v 'jpg' |  httpx -silent -mc 200,301 -o resultAll.txt
7.cat target.txt | assetfinder -subs-only |grep -v 'static\|img' |anew | waybackurls| anew| grep -v 'jpg' |  httpx -silent -mc 200,301 -o resultAll.txt
```

(2)

**gospider爬虫的用法**

```
gospider -S targets1.txt -o output -c 30 -d 3 --blacklist ".(jpg|jpeg|gif|css|tif|tiff|png|ttf|woff|woff2|ico)"

gospider -s "https://google.com/" -o output -c 10 -d 1 --other-source --include-subs
```

```
gospider -s https://twitch.tv --js | grep -E "\.js(?:onp?)?$" | awk '{print $4}' | tr -d "[]" | anew | anti-burl

cat gospider | pcregrep -oP "http(s)?://((?i)(([a-zA-Z0-9]{1}|[_a-zA-Z0-9]{1}[_a-zA-Z0-9-]{0,61}[a-zA-Z0-9]{1})[.]{1})+)?ROOTDOMAIN.*"

提取URL:
cat *_* | grep -ohr -E "https?://[a-zA-Z0-9\.\/_&=@$%?~#-]*" |
```

用于:批量扫描Key泄露

```
cat 200.txt| xargs -I %% SecretFinder -i %% -o cli
```



批量扫描js文件:

JSFSCAN.h

>```
>docker run -v $(pwd):/jsfscan -it jsfscan  "/bin/bash"
>```

```
1.sed   "s/^/http:\/\//g"  targets.txt > targets1.txt
2.bash JSFScan.sh -l target.txt  --all -r -o target.ru
```





批量扫描域名接管

- git clone https://github.com/r3curs1v3-pr0xy/sub404.git
- Install dependencies: pip install -r requirements.txt
- Install [Subfinder](https://github.com/projectdiscovery/subfinder) (optional)
- Install [Sublist3r](https://github.com/aboul3la/Sublist3r) (optional)
- python3 sub404.py -h

example:

```
 sub404 -d qq.com
```



一体化扫描器reconftw:

```
INSTALL:
cd Docker && docker build -t reconftw .
docker run -it -v  /Volumes/windowSSD/挖洞工具类/reconftw:/root/Tools/reconftw reconftw /bin/bash

docker run -it -p 8081:80   -v  /Volumes/windowSSD/挖洞工具类/reconftw:/root/Tools/reconftw reconftw  registry.cn-beijing.aliyuncs.com/xq17/reconftw:v1  /bin/bash

docker run -it -p 8081:80   -v  /Volumes/windowSSD/挖洞工具类/reconftw:/root/Tools/reconftw  reconftw  /bin/bash

挂代理使用:export all_proxy=socks5://127.0.0.1:7890

USAGE:
./reconftw.sh -d example.com -r
mkdir  /root/Tools/reconftw
./reconftw.sh -l target.txt -r -o  /output/directory/
./reconftw.sh -l target.txt -a --deep -o  /output/directory/
```

```
docker pull registry.cn-beijing.aliyuncs.com/xq17/reconftw:v1
docker run -it -p 8081:80  -v $(pwd)/output:/root/Tools/reconftw/Recon/ registry.cn-beijing.aliyuncs.com/xq17/reconftw:v1 /bin/bash
./reconftw.sh -d target.com -r --deep -o /root/Tools/reconftw/Recon/
./reconftw.sh -l target.txt -r --deep -o /root/Tools/reconftw/Recon/output
```

其他细节:https://github.com/six2dez/reconftw



批量扫描Dom/reflecrt XSS

```
xargs -a domain -I@ sh -c 'wingman -u @ --crawl | notify
```

安装:

```
wget https://xsswingman.com/downloads/wingman/wingman_darwin_amd64.zip
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



### 收集子域名

1.amass 收集

```
amass enum -d target.com -o /filepath/subdomains.txt
sort -u subdomains.txt | httprobe > /filepath/uniq.txt
```

2.多种扫描方式集合docker镜像

```
sudo docker run -it --rm -v /home/ubuntu/results/:/root/results/ uexpl0it/subdomains-enumerator:0.4
```

3.第三方api

```
HOST=immomo.com && curl --silent --insecure "https://sonar.omnisint.io/subdomains/${HOST}" | grep -oE "[a-zA-Z0-9._-]+\.${HOST}"
```

4.验证+去重



```
1.curl "https://public-dns.info/nameservers.txt" -L -o resolver.txt
2.puredns resolve baidu_all_sub.txt -r resolver.txt  --write valid_domains.txt \
                            --write-wildcards wildcards.txt \
                            --write-massdns massdns.txt
```





## 请求方式脆弱点

```
GET /api/users/123/profile

Try:
GET       /api/users/123/profile
POST     /api/users/123/profile
PUT        /api/users/123/profile
PATCH    /api/users/123/profile
DELETE  /api/users/123/profile
```







## OneLiner

https://github.com/twseptian/oneliner-bugbounty



## 在线工具

生成反弹shell: https://www.revshells.com/





## 测试payload

这个很多骚姿势: [Payloads_xss_sql_bypass](https://github.com/Y000o/Payloads_xss_sql_bypass)

