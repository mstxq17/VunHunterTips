# SSRF 漏洞挖掘技巧

## 0x1 自动化挖掘SSRF

### 0x1.1 组合思路

![Et9d-mlXYAE7cg0](常用的SSRF参数.assets/Et9d-mlXYAE7cg0-3134197.jpeg)

### 1.2 burp插件SSRF-king

```
git clone https://github.com/ethicalhackingplayground/ssrf-king
gradle build
```

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

