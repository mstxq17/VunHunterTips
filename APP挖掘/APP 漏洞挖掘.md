## APP 漏洞挖掘

### 敏感信息泄露

### 自动化工具

```
$ git clone https://github.com/dwisiswant0/apkleaks
$ cd apkleaks/
$ pip3 install -r requirements.txt
```

实例:

```
1.wget https://a.app.qq.com/o/simple.jsp?pkgname=reader.com.xmly.xmlyreader&channel=0002160650432d595942&fromcase=60001
2. python3 apkleaks.py -f /Users/xq17/Downloads/Misc/reader.com.xmly.xmlyreader_2.4.04_64.apk
```

