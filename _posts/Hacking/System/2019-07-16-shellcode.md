---
title : "쉘코드 만들기"
excerpt : "쉘코드 만들기"
toc : true
toc_sticky : true
categories :
  - System
  - Linux
  - BOF
tags :
  - shellcode 만들기
---

## 1. system("/bin/sh")

system("/bin/sh")에서 system 함수는 내부적으론 execve 함수를 호출하게되고 execve 함수는 또 내부적으로 sys_execve라는 system call을 통해서 쉘을 실행시키는 구조임.  
그래서 마지막의 sys_execve 부분만 복원을 해주면 됨.

## 2. 어셈블리어로 작성 후 
리눅스에서의 sys_execve 함수
```
|   Name     |    eax     |     ebx       |        ecx             |           edx          |       esi         |
| sys_execve |    0x0b	  | char __user * |	char __user *__user *	 |  char __user *__user * |  struct pt_regs * |
```

gets(), strcpy()에 전달하기 위해

