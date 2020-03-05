---
title : "Websec - Level 11"
excerpt : "Websec - Easy Level 11 (풀이봄..)"
toc: true
toc_sticky: true
categories:
  - Websec
tags:
  - sql injection
---

## 1. Source code
<img src="/assets/images/websec-level 11.jpg">
``` php
function sanitize($id, $table) {
    if (! is_numeric ($id) or $id < 2) {
        exit("The id must be numeric, and superior to one.");
    }

    $special1 = ["!", "\"", "#", "$", "%", "&", "'", "*", "+", "-"];
    $special2 = [".", "/", ":", ";", "<", "=", ">", "?", "@", "[", "\\", "]"];
    $special3 = ["^", "_", "`", "{", "|", "}"];
    $sql = ["union", "0", "join", "as"];
    $blacklist = array_merge ($special1, $special2, $special3, $sql);
    foreach ($blacklist as $value) {
        if (stripos($table, $value) !== false)
            exit("Presence of '" . $value . "' detected: abort, abort, abort!\n");
    }
}

if (isset ($_POST['submit']) && isset ($_POST['user_id']) && isset ($_POST['table'])) {
    $id = $_POST['user_id'];
    $table = $_POST['table'];

    sanitize($id, $table);

    $pdo = new SQLite3('database.db', SQLITE3_OPEN_READONLY);
    $query = 'SELECT id,username FROM ' . $table . ' WHERE id = ' . $id;
    //$query = 'SELECT id,username,enemy FROM ' . $table . ' WHERE id = ' . $id; // enemy에 flag값이 들어있다.

    $getUsers = $pdo->query($query);
    $users = $getUsers->fetchArray(SQLITE3_ASSOC);

    $userDetails = false;
    if ($users) {
        $userDetails = $users;
    $userDetails['table'] = htmlentities($table);
    }
}

The hero number <strong><?php echo $userDetails['id']; ?></strong>
in <strong><?php echo $userDetails['table']; ?></strong>
is <strong><?php echo $userDetails['username']; ?></strong>.
```

## 2. Vulnerability
문제 설명을 보면 AS가 강조되어 있다고 함. as는 별명을 붙여줄 때 사용하는데 as가 없어도 별명을 붙일 수 있음. table 부분에 서브쿼리를 줘서 플래그 값을 볼 수 있음.  


## 3. Solution
풀이부터 보여주면 ```(select 2 id,enemy username from costume where id like 1) ```을 table 값으로 주고 id값으로 2를 주면 플래그가 나옴.   
**완성된 쿼리**: ```SELECT id,username FROM (select 2 id,enemy username FROM costume where id like 1) WHERE id = 2```  
처음에 풀이를 보고서도 이해가 안돼서 직접 해보았음. 일단 test 테이블임.
<img src="/assets/images/websec-level 11 as test1.jpg">  

보통은 별명을 지어주면 결과를 출력할 때 별명이 컬럼명이 되는걸로 알고 있는데 숫자에다 별명을 지어주면 어떻게 되는지 해보았음.  

<img src="/assets/images/websec-level 11 as test2.jpg">  

오히려 숫자가 그냥 컬럼의 값으로 들어가 버림. 즉, ```select 2 id,enemy username from costume where id like 1``` 이 쿼리문으로 보면 id 값에는 2가 들어가게 되고 enemy 값이 username 컬럼명으로 바뀌어서 출력이 되므로 username 부분에서 플래그가 나옴. 