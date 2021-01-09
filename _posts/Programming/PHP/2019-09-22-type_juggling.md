---
title : "PHP type juggling"
excerpt: "type juggling"
toc: true
toc_sticky: true
categories:
    - PHP
tags :
    - type juggling
---

## 자동형변환(Type Juggling)
```
대입 연산자를 기준으로 오른쪽에서 왼쪽으로 자동 형 변환.
메모리 크기가 작은 자료형에서 큰 자료형으로 변환되는 것을 우선순위로 둠.

형변환을 했을 때, 문자열이거나 없다거나 실패하면 0을 리턴하고 아니라면 가능한 범위까지 형변환함. 

ex) 'admin*99 --> return 0, '123admin'*10 --> return 1230
```

자세한 건 
<a href="https://blog.lael.be/post/1993" target="_blank">https://blog.lael.be/post/1993</a>  
<a href="https://moolgogiheart.tistory.com/58" target="_blank">https://moolgogiheart.tistory.com/58</a>  
