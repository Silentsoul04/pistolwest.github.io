---
title : "MySQL functions"
excerpt: "MySQL cheat sheet"
toc: true
categories:
  - PHP
tags :
  - MySQL
---

## Cheat Sheet
<a href="https://overapi.com/" target="_blank">https://overapi.com/</a>  

## 써 본 것 정리

### 사용자 정의 변수
```
MySQL에서 하나의 명령문에 있는 값을 다른 명령문으로 전달하는 것이 가능.
사용자 정의 변수에 값을 저장한 후에 나중에 그 값을 참조 가능.

한 클라이언트에서 정의된 사용자 변수는 다른 클라이언트에서는 보이지 않으며 사용 불가. 
해당 커넥션에서 정의된 모든 변수는 클라이언트가 종료되면 자동으로 해제된다.

사용자 변수는 @var_name 이라고 쓰며, 항상 @ 하나가 따라와야 한다.
사용자 변수 이름은 알파벳, 숫자, ‘.’, ‘_’, 그리고 ‘$’을 사용할 수 있다.
```
```
사용자 변수 값 할당 방법

SET @var_name = expr  (정수, 실수, 문자열, NULL) 
SET 변수할당은 = 또는 := 둘다 사용 가능.

비 SET 문에서는 := 로 할당. = 는 비교 연산자로 취급되기 때문이다.

여러개의 변수를 선언할때는 DECLARE @ 변수명 자료형, @변수명 자료형
```
출처: <a href="https://link2me.tistory.com/536" target="_blank">[소소한 일상 및 업무TIP 다루기]</a>

### union
```
유니온의 규칙

하나의 ORDER BY만 사용할 수 있다.
각 SELECT의 열수, 표현식이 같아야 한다.
SELECT 문들 끼리 순서는 상관없다.
유니온을 한 결과가 중복되면 하나만 나온다. (DEFAULT)
열의 타입은 같거나 반환 가능한 형태여야 한다.
중복값을 나타내고 싶다면 UNION ALL

결과 테이블의 컬럼 명은 앞 쪽 명령문에 있는 컬럼 명을 따름.
```
출처: <a href="https://futurists.tistory.com/18" target="_blank">[미래학자]</a>

### 서브쿼리
```
() 안에 sql문 작성

서브 쿼리의 결과는 한 개 보다 크다면 오류가 발생.
```

### join
![](https://t1.daumcdn.net/cfile/tistory/99219C345BE91A7E32){: .align-center}
위 사진과 아래 블로그로 보는게 더 나음.
출처: <a href="https://yoo-hyeok.tistory.com/98" target="_blank">https://yoo-hyeok.tistory.com/98</a>

### limit
```
LIMIT [num] : 처음부터 num개의 행을 가져옴.

LIMIT 시작점, 갯수 (첫번째 파라미터는 0 부터 시작)
```

### substr, mid
```
SUBSTR(str,시작점), SUBSTR(str FROM 시작점), SUBSTR(str,시작점,len)
SUBSTR(str FROM 시작점 FOR len)

시작점와 len은 1부터 시작, 0이면 empty string 출력. 

SUBSTR은 SUBSTRING와 동의어.
SUBSTR 대신 SUBSTRING으로 해도 됨.

MID(str,시작점,len), mid 또한 동의어.
```

### as
```
쿼리안에 테이블 혹은 서브쿼리 에 AS로 별명 짓기 가능.
정해준 이름으로 쿼리 안에서 사용 가능.

별칭에 공백이 있으면 따옴표 사용. '별칭'

select 컬럼명 as 별칭

select 함수 as 별칭

select (서브쿼리) as 별칭

as 생략 가능

select col1 as name from table
```

### like
```
expr LIKE pat [ESCAPE 'escape_char']
expr NOT LIKE pat [ESCAPE 'escape_char']

두 개의 와일드카드 %, _가 있음.
% : 모든 문자열
_ : 한 글자

LIKE 연산으로 '%'나 '_'가 들어간 문자를 검색하기 위해서는 ESCAPE를 사용해야 한다. 
'_'나 '%'앞에 ESCAPE로 특수 문자를 지정하면 검색 가능.
ESCAPE는 escape 문자 \ 대신에 사용하 문자를 지정하는 것.
```
```
select 'abc' like 'a%' -> 1
select 'abc' like '%c' -> 1
select 'abc' like '%b' -> 0
```
```
select 'abc' like 'ab_' -> 1
select 'abc' like 'a__' -> 1
select 'abc' like '_a_' -> 0
```

### IN
```
보통 아래와 같이 한개 필드에 여러 값을 검색할 경우 IN() 함수를 이용하여 검색.

select * from table where (col1, col2, ..., col@) IN(val1, val2, ..., val@)
```
```
IN 구문에 서브쿼리 사용 가능하지만 추천 x -> 사용시간이 오래 걸림.
자세한 설명은 https://jojoldu.tistory.com/520

결론 : 버전/조건 관계 없이 좋은 성능을 내려면 최대한 Join을 이용하자
```

### CONCAT
```
CONCAT(str1,str2,...)
-> 둘 이상의 문자열을 입력한 순서대로 합쳐서 반환해주는 함수
-> NULL이 포함되면 NULL 리턴

mysql> SELECT CONCAT('My', 'S', 'QL');
        -> 'MySQL'
mysql> SELECT CONCAT('My', NULL, 'QL');
        -> NULL
mysql> SELECT CONCAT(14.3);
        -> '14.3'
mysql> SELECT 'My' 'S' 'QL';
        -> 'MySQL'        
```

### GROUP_CONCAT 
```
GROUP_CONCAT([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [SEPARATOR str_val])

SEPERATOR default ','
합쳐지는 문자열에 중복되는 문자열을 제거 할때는 distinct 를 사용
문자열을 정렬하여 나타내고 싶으면 order by 를 이용

-> 그룹화된 컬럼의 행들을 하나의 행으로 보여줌
```
```
select * from test 
```
```
|  type 	|    name    	|
| fruit 	|    banana   	|
| fruit 	|    apple   	|
| fruit 	| watermelon 	|
| fruit 	|    apple   	|
```
기본적인 group_concat() 사용.
```
select type, group_concat(name) from test group by type 
```
```
|  type 	|               name               	|
| fruit 	| banana, apple, watermelon, apple 	|
```
구분자를 변경하고 싶으면 **seperator** 사용.
```
select type, group_concat(name separator '|') from test group by type
```
```
|  type 	|               name               	|
| fruit 	| banana| apple| watermelon| apple 	|
```
중복되는 문자열을 제거할 때 **distinct** 사용.
```
select type, group_concat(distinct name) from test group by type
```
```
|  type 	|               name            |
| fruit 	| banana, apple, watermelon 	|
```
문자열을 정렬하고 싶으면 **order by** 사용.
```
select type, group_concat(distinct name order by name) from test group by type
```
```
|  type 	|               name               	|
| fruit 	| apple, banana, watermelon 	    |
```
자세한건 <a href="https://fruitdev.tistory.com/16" target="_blank">https://fruitdev.tistory.com/16</a>

### GROUP BY ... HAVING ...
```
GROUP BY column1 [, column2 ...] [HAVING condition]
-> 컬럼을 그룹화하여 검색
-> 보통 집합 함수와 사용됨. ex) AVG, SUM, COUNT

```
자세한 건 <a href="https://extbrain.tistory.com/56" target="_blank">https://extbrain.tistory.com/56</a>

### ORDER BY ... ASC/DESC
```
ORDER BY column [ASC/DESC]
-> 테이블 조회 정렬, 기본 값은 오름차순

ORDER BY column1 [, column2, ..] 
-> 여러 컬럼 정렬

ORDER BY 컬럼 번호1 [, 컬럼 번호2, 컬럼 번호3 ...];
-> 컬럼 번호로 정렬

ORDER BY 컬럼 = '값'
=, != 를 통해서 값을 비교할 경우 
1. 해당 값은 오름차순일 경우 가장 마지막에 출력. 
2. 내림차순일 경우 가장 처음에 출력이 된다.
```
```
SELECT name, age FROM table_name ORDER BY age DESC, name
-> age 내림차순, name 오름차순 정렬

SELECT name, age FROM table_name ORDER BY 1 DESC, 2
-> name 오름차순, age 내림차순 정렬
```
출처 : <a href="https://extbrain.tistory.com/56" target="_blank">https://extbrain.tistory.com/51</a>  
출처 : <a href="https://blog.do9.kr/132" target="_blank">https://blog.do9.kr/132</a>

### IF 
```
IF(expr1,expr2,expr3)
-> expr1이 true면 expr2 리턴, false면 expr3 리턴

mysql> SELECT IF(1>2,2,3);
        -> 3
mysql> SELECT IF(1<2,'yes','no');
        -> 'yes'
mysql> SELECT IF(STRCMP('test','test1'),'no','yes');
        -> 'no'
```

### CASE
```
CASE value WHEN compare_value THEN result1 [WHEN compare_value THEN result ...] [ELSE result2] END
-> value를 compare_value와 비교하여 true면 result1 리턴, 그 외엔 result2 리턴

CASE WHEN condition THEN result1 [WHEN condition THEN result ...] [ELSE result2] END
-> condition이 참일 때, result1 리턴
-> 비교 또는 조건이 참이 아닌 경우, result2를 리턴
-> else 문이 없다면 NULL 리턴


mysql> SELECT CASE 1 WHEN 1 THEN 'one' WHEN 2 THEN 'two' ELSE 'more' END;
        -> 'one'
mysql> SELECT CASE WHEN 1>0 THEN 'true' ELSE 'false' END;
        -> 'true'
mysql> SELECT CASE BINARY 'B' WHEN 'a' THEN 1 WHEN 'b' THEN 2 END;
        -> NULL
```

### BETWEEN ... AND ...
```
expr BETWEEN min AND max
-> min <= expr => max 이면 1, 아니면 0 리턴.


mysql> SELECT 2 BETWEEN 1 AND 3, 2 BETWEEN 3 and 1;
        -> 1, 0
mysql> SELECT 1 BETWEEN 2 AND 3;
        -> 0
mysql> SELECT 'b' BETWEEN 'a' AND 'c';
        -> 1
mysql> SELECT 2 BETWEEN 2 AND '3';
        -> 1
mysql> SELECT 2 BETWEEN 2 AND 'x-3';
        -> 0
```

### INSTR
```
INSTR(str,substr), locate(substr,str)와 동일
-> str에 있는 substr의 첫 인덱스를 찾음.

mysql> SELECT INSTR('foobarbar', 'bar');
        -> 4
mysql> SELECT INSTR('xbar', 'foobar');
        -> 0
```

### LOCATE
```
LOCATE(substr,str), LOCATE(substr,str,시작점)
-> str에 있는 substr의 첫 인덱스를 찾음.
-> str의 시작점에서부터 substr의 첫 인덱스를 찾음.

mysql> SELECT LOCATE('bar', 'foobarbar');
        -> 4
mysql> SELECT LOCATE('xbar', 'foobar');
        -> 0
mysql> SELECT LOCATE('bar', 'foobarbar', 5);
        -> 7
```

### SLEEP
```
SLEEP(duration)
-> 레코드 당 정해진 초 동안 지연
-> 정상종료 시 0, 그 외에는 1 리턴

mysql> SELECT SLEEP(1000);
        -> 0
```

### BENCHMARK
```
BENCHMARK(count,expr)
-> expr을 count만큼 반복
-> Mysql이 expr을 얼마나 빨리 실행하는지 시간을 재며, 항상 0을 리턴.

mysql> SELECT BENCHMARK(1000000,AES_ENCRYPT('hello','goodbye'));
+---------------------------------------------------+
| BENCHMARK(1000000,AES_ENCRYPT('hello','goodbye')) |
+---------------------------------------------------+
|                                                 0 |
+---------------------------------------------------+
1 row in set (4.74 sec)
```

### DATABASE, SCHEMA
```
DATABASE() 또는 SCHEMA()
-> 현재 데이터베이스 이름 리턴. 
-> 기본 데이터베이스가 없으면 NULL 리턴.

mysql> SELECT DATABASE();
        -> 'test'
```

### VERSION
```
VERSION()
-> mysql 버젼 리턴

mysql> SELECT VERSION();
        -> '8.0.24-standard'
```

### USER, SYSTEM_USER, SESSION_USER
```
USER() == SYSTEM_USER() == SESSION_USER()
-> 'utf-8'로 mysql 유저와 호스트 이름 리턴

mysql> SELECT USER();
        -> 'davida@localhost'
```
