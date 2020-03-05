---
title : "Websec - Level 17"
excerpt : "Websec Level 17 - Babysteps"
toc : true
toc_sticky: true
categories:
  - Websec
tags:
  - PHP loose comparison vulnerability
---

## 1. Source code

``` php
include "flag.php";

function sleep_rand() { /* I wish php5 had random_int() */
        $range = 100000;
        $bytes = (int) (log($range, 2) / 8) + 1;
        do {  /* Side effect: more random cpu cycles wasted ;) */
            $rnd = hexdec(bin2hex(openssl_random_pseudo_bytes($bytes)));
        } while ($rnd >= $range);
        usleep($rnd);
}

if (isset ($_POST['flag'])):
    sleep_rand(); /* This makes timing-attack impractical. */
                                                 
if (! strcasecmp ($_POST['flag'], $flag))
    echo '<div class="alert alert-success">Here is your flag: <mark>' . $flag . '</mark>.</div>';   
else
    echo '<div class="alert alert-danger">Invalid flag, sorry.</div>';
```

## 2. Vulnerability
strcasecmp는 strcmp함수랑 똑같은 함수임. 그냥 ==이 빠진거. [strcmp() 취약점](https://hackability.kr/entry/PHP-strcmp-
%EC%B7%A8%EC%95%BD%EC%A0%90%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%9D%B8%EC%A6%9D-%EC%9A%B0%ED%9A%8C)  

정리하면 PHP 5.3 버전에서 발생하는 문제로 인자값에 배열을 넣게되면 NULL 값을 반환해서 PHP loose comparison vulnerability에 의해 true를 리턴해서 우회가 가능함.

"==" 으로 비교할 때
![](https://t1.daumcdn.net/cfile/tistory/2478193352ED08F92B){: .align-center}

"===" 으로 비교할 때
![](https://t1.daumcdn.net/cfile/tistory/211D203752ED093D26){: .align-center}

## 3. Solution

burp suite를 이용해서 flag값을 배열로 바꿔서 주면 된다. ```flag[]=123```  
