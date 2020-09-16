---
title : "Directory traversal"
excerpt : "Root-me.org Web-Server Directory traversal"
toc: true
toc_sticky: true
categories :
  - Web-Server
---

# Directory traversal
```
Author
g0uZ,  31 July 2011

Statement
Find the hidden section of the photo galery.
```
문제 들어가면 여러가지 디렉토리들이 있음. emote, apps, device 등등..  
처음엔 LFI인줄 알고 ```../../etc/passwd``` 시도했고 실패했지만 file_exist()함수를 사용한다는 걸 발견.  
여기서 file_exist()함수에 이미 경로가 ```galerie/../../etc/passwd``` 이렇게 되어있었다는 점.  
그래서 galerie에 아무 값도 주지 않으니 숨겨진 디렉토리가 나옴. ```86hwnX2r```  
저 값을 인자로 주면 password가 나옴. 
```
flag : kcb$!Bx@v4Gs9Ez 
```