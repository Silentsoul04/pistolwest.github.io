---
title : "Backup file"
excerpt : "Root-me.org Web-Server Backup file, 풀이 봄"
toc: true
toc_sticky: true
categories :
  - Web-Server
---

# Backup file
```
Author
g0uZ,  27 February 2011

Statement
No clue.
```
풀이를 찾아보니 http-backup-finder라는게 있다고 함. nmap을 이용하면 됨.  
```
nmap -p 80 --script=http-backup-finder --script-args http-backup-finder.url=/web-serveur/ch11/index.php challenge01.root-me.org

Starting Nmap 7.60 ( https://nmap.org ) at 2020-06-12 00:46 KST
Nmap scan report for challenge01.root-me.org (212.129.38.224)
Host is up (0.32s latency).
Other addresses for challenge01.root-me.org (not scanned): 2001:bc8:35b0:c166::151

PORT   STATE SERVICE
80/tcp open  http
| http-backup-finder:
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=challenge01.root-me.org
|_  http://challenge01.root-me.org:80/web-serveur/ch11/index.php~

Nmap done: 1 IP address (1 host up) scanned in 12.76 seconds
```
