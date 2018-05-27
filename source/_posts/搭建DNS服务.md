---
title: 搭建DNS服务
date: 2018-05-27 18:07:00
tags:
---

使用 dnsmasq 保证本地 DNS 服务的稳定性(主要看这个)  
http://www.sunzhongwei.com/use-dnsmasq-ensure-dns-stable.html

参考   
http://www.360doc.com/content/14/0913/13/8314158_409140713.shtml    
http://www.knowsky.com/888297.html

```
# 启动
sudo /etc/init.d/dnsmasq restart

# 检查状态
sudo service dnsmasq status

# 最终检查
nslookup cpsbk.guang.com

```
