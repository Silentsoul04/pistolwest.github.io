---
title : "Stereotyped Challenge 1 - social_info"
excerpt : "Stereotyped Challenge 1 - social_info"
toc: true
toc_sticky: true
categories :
  - Stereotyped Challenge
---

# social_info
```
stypr is developing a website to promote cyber security for his undergradute friends.
Hackers need to know where it is. I heard that it's somewhere hidden in *.stypr.com
```
```.stypr.com```을 검색하면 harold.kim으로 이동함. 
백업파일을 다운받아 확인하면 됨.  
```
nmap -p 80 -script=http-backup-finder --script-args http-backup-finder.url=index.php harold.kim


Starting Nmap 7.60 ( https://nmap.org ) at 2020-06-12 01:49 KST
Nmap scan report for harold.kim (175.215.217.98)
Host is up (0.016s latency).
rDNS record for 175.215.217.98: stypr.com

PORT   STATE SERVICE
80/tcp open  http
| http-backup-finder:
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=harold.kim
|_  https://harold.kim/index.php~

Nmap done: 1 IP address (1 host up) scanned in 0.97 seconds
```

```
flag{9688dc92b6500ab6eafb68589ed556cb}
```