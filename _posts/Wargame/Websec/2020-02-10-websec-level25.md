---
title : "Websec - Level 25"
excerpt : "Websec Level 25 - Babysteps"
toc : true
toc_sticky: true
categories:
  - Websec
tags:
  - parse_url() vulnerability
---

## 1. Source code

``` php
parse_str(parse_url($_SERVER['REQUEST_URI'])['query'], $query);
foreach ($query as $k => $v) {
    if (stripos($v, 'flag') !== false)
        die('You are not allowed to get the flag, sorry :/');
}
include $_GET['page'] . '.txt';
```

## 2. Vulnerability

플래그 흭득 조건은 if문을 통과를 해야 flag.txt 파일을 include 시켜서 플래그를 흭득할 수 있음.  
우선 parse_url()함수란? 자세한 건 여기서 [parse_url()](http://b.redinfo.co.kr/65)  
요약하면 **parse_url()**는 URL 형태의 문자열을 받아와서 scheme,host, port, user, pass, path, query, fragment 의 값을 반환하는데 URL 문자열에 있는 값만 반환함.  
**parse_str()**는 *이름=값&이름=값(쿼리 형태)*의 값을 각각의 변수로 만들어주는 함수임.  
**stripos()**는 문자열을 찾는 함수임.  
취약점은 parse_url()에서 발생을 함.

나도 직접 확인을 해보았음.  
**우선 정상적인 URL일때는**  
```URL: http://websec.fr/level25/index.php?page=main```  
```Array ( [scheme] => http [host] => websec.fr [path] => /level25/index.php [query] => page=main )```    
```URL: /a.php?get=a => host: NULL , path: string(6) "/a.php" , query: string(5) "get=a" ```  
**하지만 비정상적일 때**  
```URL: //a.php?get=a  => host: string(5) "a.php" , path: NULL , query: string(5) "get=a"  ```  
보통은 // 다음엔 host부문이라서 a.php를 host로 인식함.  
```URL: ///a.php?get=a  => host: NULL , path: NULL , query: NULL  ) ```  
URL에는 slash가 3개인 경우는 없어서 값이 NULL임.


## 3. Solution

문제에서는 query값에 flag가 없으면 되므로 slash를 3개를 쓰면은 if문을 통과하므로 flag.txt 파일을 볼 수 있다.