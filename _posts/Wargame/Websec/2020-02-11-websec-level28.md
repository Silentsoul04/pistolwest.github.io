---
title : "Websec - Level 28"
excerpt : "Websec Level 28 - Babysteps"
toc : true
toc_sticky: true
categories:
  - Websec
tags:
  - race condition
---

## 1. Source code

``` php
if(isset($_POST['submit'])) {
  if ($_FILES['flag_file']['size'] > 4096) {
    die('Your file is too heavy.');
  }
  $filename = md5($_SERVER['REMOTE_ADDR']) . '.php';

  $fp = fopen($_FILES['flag_file']['tmp_name'], 'r'); # 서버에 저장된 파일을 읽기모드로 열기
  $flagfilecontent = fread($fp, filesize($_FILES['flag_file']['tmp_name']));
  @fclose($fp);

  file_put_contents($filename, $flagfilecontent);
  if (md5_file($filename) === md5_file('flag.php') && $_POST['checksum'] == crc32($_POST['checksum'])) {
    include($filename);  // it contains the `$flag` variable
    } 
  else {
    $flag = "Nope, $filename is not the right file, sorry.";
    sleep(1);  // Deter bruteforce
  }

  unlink($filename);
}
```

## 2. Vulnerability

우선 우리는 파일에 대해 검사를 하는 if문을 만족시킬 수 없으므로 우회를 해야함. else 문을 보면 sleep(1) 코드가 있고 else문이 끝나고 unlink를 함. 즉, 1초 약간 넘는 시간동안은 파일을 볼 수 있다는 소리가 됨. 이런 기법을 **race condition**이라고 함. 


## 3. Solution

