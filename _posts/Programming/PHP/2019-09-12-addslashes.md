---
title : "PHP - addslashes()"
excerpt : addslashes()
toc : true
toc_sticky : true
categories :
    - PHP
---

## addslashes()
```php
addslashes ( string $string ) : string

따옴표, 쌍따옴표, 백슬래쉬, NULL 문자에 대해 escape 해주는 함수 

$str = "Is your name O'Reilly?";

echo addslashes($str) // Outputs: Is your name O\'Reilly?
```