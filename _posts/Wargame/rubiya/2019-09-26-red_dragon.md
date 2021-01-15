---
title : "LOS Lv.26 red_dragon (풀이 봄)"
excerpt : "LOS Lv.26 red_dragon"
toc: true
toc_sticky: true
categories :
  - rubiya
---


## red_dragon
```php
query : select id from prob_red_dragon where id='' and no=1

<?php
  include "./config.php";
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\./i', $_GET['id'])) exit("No Hack ~_~");
  if(strlen($_GET['id']) > 7) exit("too long string");
  $no = is_numeric($_GET['no']) ? $_GET['no'] : 1;
  $query = "select id from prob_red_dragon where id='{$_GET['id']}' and no={$no}";
  echo "<hr>query : <strong>{$query}</strong><hr><br>";
  $result = @mysqli_fetch_array(mysqli_query($db,$query));
  if($result['id']) echo "<h2>Hello {$result['id']}</h2>";

  $query = "select no from prob_red_dragon where id='admin'"; // if you think challenge got wrong, look column name again.
  $result = @mysqli_fetch_array(mysqli_query($db,$query));
  if($result['no'] === $_GET['no']) solve("red_dragon");
  highlight_file(__FILE__);
?>
```

## Solution
```
?id='||no>&no=%0a1 이런식으로 개행문자를 이용해서 해결 가능..
```
