---
layout: post
title:  "제작한 웹 페이지 포트 포워딩 하기"
date:   2021-12-1 14:05:21 +0800
tags: EC2 PORT_FORWARDING 
color: rgb(255,90,90)
cover: ''
subtitle: '포트포워딩 시키기'
---

## 포트포워딩 명령어 (ver. LINUX)

```
sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
```

