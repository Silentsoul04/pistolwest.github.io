---
title: "Linux 명령어"
excerpt: "Linux 명령어"
toc: true

categories:
  - Linux
tags :
  - Linux Commands
---

## 리눅스 기초 특수기호 문자
```
‘>‘ 표준 출력 리다이렉션 : 출력 방향을 바꿈.

ex) ls > test.txt : 출력 내용을 test.txt 파일에 기록한다. //파일이 없다면 생성
ex) cat > test.txt : 내용을 입력한 후 입력 내용을 test.txt 파일에 저장한다.

‘>>’ 이면 ls >> test.txt는 출력된 내용을 test.txt 파일에 덧붙인다. //이어쓰기
```
```
‘<’ : 표준 입력

ex) cat < test.txt : test.txt.의 내용을 cat 명령어로 읽은 뒤 화면에 출력
```
```
'*' : 모든 문자와 일치하는 와일드 카드 문자

ex) ls tes* : test.txt, tes/123.txt 등 일치하는 모든 파일/디렉토리(내부)가 출력된다.

(+) 파일 이름 자리에 *를 적으면 모든 파일을 뜻함. ex) ls * ==> 모든 파일 보기

'?' : 하나의 문자와 일치하는 와일드 카드 문자 //길이가 1인 임의의 문자
      ex) ls test.tx? : test.txt, test.txx 등 하나 일치한 파일을 출력한다.

‘[ ]‘ 은 괄호 안에 포함된 문자 중 하나라도 일치되는 문자.
ex) ls a[13] : a1 , a3 파일 목록 출력.

; : 명령어 분리자로 한 명령 라인에서 여러 가지 명령을 수행할 수 있도록 함

$ : 쉘 변수를 가르킴. 

` : 명령 대체, 큰따옴표안에서 명령어들이 문자열이 아닌 명령어의 역할을 하도록 해준다.

| : 앞에 나온 명령어의 표준 출력 결과를 다음 명령어의 표준 입력으로 사용
    ex) ls –al | grep ^d : 모든 파일을 출력하는데 d로 시작하는 파일을 출력.

‘ ’ , “ ” : 문자를 감싸서 문자열로 만들고 문자열 안의 특수기호 기능도 없앰.

작은따옴표는 모든 특수기호, 큰 따옴표는 $,`,\를 제외한 모든 특수기호를 일반문자로 간주

` ` : 셸은 ` ` 로 감싸인 문자열을 명령으로 해석하게 함.

\ : 특수문자 앞에 사용, 특수 문자의 효과를 없애고 일반 문자처럼 처리함.
```

## 기본 명령어
```
rm-rf <file name> : file 강제 삭제

cp-r(복사할 디렉터리 경로) (붙여넣을 디렉터리 경로) //파일 복사임.

==> cp-r /etc/mnt

디렉터리 생성

mkdir –p dir01/dir02 : dir01 디렉터리 안에 dir02디렉터리까지 같이 생성

bin : binary파일 , 실행 파일
mnt,tmp: 임시파일
etc : 설정파일

head [file] : 파일의 앞부분부터 확인, 기본적으로 처음 10행 출력
head -n [num] [file] : 앞부분에서 num행까지 출력
head -c [num] [file] : num byte까지의 내용 출력

tail [file] : 파일의 끝부분부터 확인, 기본적으로 마지막 10행을 출력
tail -n [num] [file]
tail -c [num] [file]
```
```
파일색깔

파랑: 디렉터리 파일
하양: 일반 파일
하늘색: 링크파일
녹색: 실행파일
빨강: 압축파일
```
```
파일 경로문자

최상위 디렉터리 : /
현재 디렉터리 : .
상위 디렉터리: ..
홈 디렉터리(root): ~
이전 작업 디렉터리 : -
```
```
경로 구분자 /

디렉터리 사이에서는 ‘/’로 경로 구분

ex) 현재 root 디렉터리의 하위에 있는 log디렉터리로 이동 시

==> cd log : 하위 디렉터리로 이동 시 에는 / 생략 가능
==> cd ./log: 현재 디렉터리 안의 log 디렉터리로 이동
==> cd log/ : 파일 이름뒤에 /붙으면 그 파일은 디렉터리란 의미
==> cd ./log/
==> cd /root/log

다 같은 의미임.
```

## man [명령어]
```
페이지 이동 방법

e : 한줄 앞으로

y : 한줄 뒤로

f : 한 window 앞으로

b : 한 window 뒤로

d : half window 앞으로

u : half window 뒤로

이전 페이지 : page up 키 or b

다음 페이지 : page down 키 or 스페이스바

종료 : q
```

## vi
```
I: 입력모드
x: 한 문자 삭제
dd: 한 라인 삭제
u: 바로 이전 상태로 되돌리기
H: 화면의 처음으로 이동
L: 화면 끝으로 이동
C; 파일의　끝으로 이동
Ctrl+f: 한 페이지 아래로 이동
ctrl+b: 한 페이지 위로 이동
yw: 커서가 있는 단어 복사
yy: 커서가 있는 라인 복사
/검색어 : 특정 단어 검색
n: 다음 단어 검색
N: 이전 단어 검색

esc: 명령 모드로 전환

‘ : ‘ : 라인 모드로 전환
w: 저장 //라인
q: 종료 //라인
q!: 저장하지 않고 종료
:sh : vi를 중단하고 쉘 실행
```

## chmod : 파일권한 변경
```
쉽게 쓰려면 8진수 형태, 복잡하지만 기능적으로 좋은 심볼릭 형태가 있음.

option :

-R : 하위 디렉토리의 모든 권한 변경

-c : 권한 변경 파일내용을 출력
```

### 8진수 형태 --> chmod [option] [8진수 퍼미션] [filename]
```
777 : 일반적인 8진수 형태
4777 : SetUid 설정, 4000을 더한다.
2777 : SetGid 설정, 2000을 더한다.
1777 : Sticky bit 설정, 1000을 더한다.

8진수 0~7은 아래와 같이 2진수로 표현이 가능하다
0 : 000
1 : 001
2 : 010
3 : 011
4 : 100
5 : 101
6 : 110
7 : 111

위 2진수 세자리는 rwx 세자리와 일치하며 2진수가 1이면 해당 권한을 부여,
0이면 해당 권한을 제거 한다.

ex) chmod 707 test.cnf 
test.cnf 파일에 대해 user, other 은 모두 rwx로 변경하고 group은 모든 권한을 제거한다.

ex) chmod 555 test.cnf 
test.cnf 파일에 대해 user, group, other 모두 rx의 권한을 주고 w의 권한은 제거한다.
```

### 심볼릭 형태 --> chmod [option] [대상] (+/-/=) (rwx) (filename)
```
대상
u : user의 권한
g : group의 권한
o : other의 권한
a : 모든 사용자 권한

+ : 해당 권한을 추가한다.
– : 해당 권한을 제거한다.
= : 해당 권한을 설정한데로 변경한다.

ex)　chmod u-x,g+r test.cnf 
test.cnf 파일에 대해 
user는 기존 권한에서 x권한만 제거한다. 나머지 권한은 그대로 유지 된다. 
group은 기존 권한에서 r권한을 추가한다. 나머지 권한은 그대로 유지 된다.

ex)　chmod u=rx,g=-,o=r test.cnf 
test.cnf 파일에 대해 user는 rx 권한만 부여, group는 모든 권한 제거, 
other은 r권한만 부여 한다
```

## chown : 파일과 그룹의 소유권을 변경하는 명령어
```
Usage : chown [option] [변경할 유저명 : 변경할 그룹명] [파일명]

option : -R : 하위 디렉토리에도 모든 권한 변경 

명령어
소유자 – 소유자만 변경한다.
:그룹명 – 그룹만 변경한다.
소유자: – 소유자와 그룹 모두 동일한걸로 변경한다.
소유자:그룹명 – 소유자와 그룹을 서로 다른걸로 변경한다. (물론 같은걸 해도 상관없다.)

파일명에는 설정을 위한 파일명 또는 디렉토리명 이용 , 와일드 카드 이용 가능

example

1) chown member1 test.cnf 
   test.cnf 파일에 대해 소유자를 member1로 바꾼다.

2) chown :member1 test.cnf 
   test.cnf 파일에 대해 그룹명을 members1로 바꾼다.

3) chown member1: test.cnf 
   test.cnf 파일에 대해 소유자 및 그룹명을 members1로 바꾼다.

4) chown member1:member2 test.cnf 
   test.cnf 파일에 대해 소유자는 member1, 그룹명은 member2로 바꾼다.
```

## find [path] [option] [action] : 주어진 조건을 검색하여 파일을 찾는다.
```
-name [name] : 지정된 이름의 파일을 찾는다.

-user [name] : user 소유의 파일을 찾는다.

-group [name] : group 소유의 파일을 찾는다.

-type [형식] : 지정된 형식의 파일을 찾는다.
• b : 블록파일
• c : 문자
• d : 디렉터리
• f : 파일
• l : 링크파일
• s : 소켓

-perm [mode] : 정확히 같은 권한을 가진 파일을 찾는다.

-perm –[mode] : mode에 있는 권한 중에서 모두 만족하는 것 출력

-perm /[mode] : mode에 있는 권한 중에서 하나만이라도 만족하는 것 출력

-size [+/-]n[bckw] : 지정된 크기의 파일을 찾는다.
• +n : n보다 크다
• -n : n보다 작다
• n : n이다
• b : 512-byte
• c : byte
• k : kilobytes
• w : 2-byte

※action
-inum number : 지정한 아이노드 번호와 파일을 찾는다.

-print : 표준출력으로 검색된 파일명을 출력한다.

-exec [command] { } \; : 찾은 각 파일에 대해 지정된 명령을 실행한다.

-ok [command] { } \; : 실행 여부를 사용자에게 확인한 후 명령을 실행한다.


2>/dev/null : find 옵션 뒤에 붙이면 에러메세지를 모두 버려주고 
              해당 조건에 정확히 부합하는 내용만을 출력해줌

너무 파일이 많고 검색된 내용도 많아서 콘솔화면에 전부 표현할 수 없다면 more을 활용.
쪽단위로 출력할 수 있게 해줍니다.
```

## grep [option] ‘pattern’ [file] : ‘pattern’을 file안에서 (옵션에 맞춰) 검색한다.
```
-A(-B) num : 일치하는 줄 아래에(위에) 지정한 줄 수(num)만큼의 내용을 더 보여준다.

-c : 일치하는 줄의 수 출력

-C [num] : 일치하는 줄의 위와 아래에 지정한 줄 수 만큼의 내용을 더 출력, 기본은 2줄

-d 디렉터리 : 읽고자 지정한 파일이 디렉터리일 경우 지정한 값을 실행한다.
 d :
    read : 디렉터리를 보통 파일처럼 읽는다. (기본 값)
    skip :  디렉터리를 건너뛴다.
    recurse(-r) : 디렉터리를 포함하여 하위 디렉터리의 모든 파일을 읽는다. 

-i : 대소문자 구별 x

-l : 일치하는 줄의 파일명만 보여주고, 줄의 내용은 출력하지 않는다.

-n : 일치하는 줄의 내용과 해당 줄의 위치를 출력한다.

-v : 지정한 패턴과 일치하지 않는 내용을 보여준다.
```

메타문자 | 기능 |      사용 예     | 사용 예 설명 
:--------:|:----:|:----------------:|:------------:
^  |  행의 시작 지시자  |  ‘^love’  |  love로 시작하는 모든 행 출력
$  |  행의 끝 지시자  |  ‘love$’  |  love로 끝나는 모든 행 출력
.  |  하나의 문자와 대응  |  ‘l..e’  |  l다음에 2문자가 오고 e로 끝나는 문자 출력
[] |  []사이의 문자 중 하나와 대응되면 출력  |  ‘[Ll]ove’  | Love나 love 문자 출력
[^]  |  []사이의 문자에 속하지 않는 문자 출력  |  ‘[^A-K]ove’  |  [A-K]ove이외의 문자 출력
\<  |  단어의 시작 지시자  |  ‘\<love’  |  love로 시작하는 단어를 포함하는 행 출력
\>  |  단어의 끝 지시자  |  ‘\>love’  |  love로 끝나는 단어를 포함하는 행 출력
x\{m\}  |  문자 x를 m번 반복  |  ‘o\{5\}’  |  문자 o가 5회 연속으로 나오는 모든 행 출력
x\{m,\}  |  문자 x를 적어도 m번 반복  |  ‘o\{5,\}’  |  문자 o가 최소한 5번 반복되는 모든 행 출력
x\{m,n\}  |  m회 이상 n회 이하 반복  |  ‘o\{5,10\}'  |  문자 o가 5회 이상 10회 이하 반복되는 모든 행 출력

```
메타문자 앞에 역슬래시를 쓰면 메타문자의 특수의미를 무시함, 즉 자체문자로 인식

ex) grep ^\.    : 마침표로 시작되는 줄을 찾음 
    
    grep '\$'   : "$" 문자가 포함된 줄을 찾음 


(1) 패턴을 작성할 때 \(역슬래쉬)가 들어가는 경우(\<, \(..\), x\{m\}) 

(2) 특수문자를 문자자체로 인식하게 하려 \을 사용하는 경우

(3) 패턴에 있어서 파이프( | )를 이용하는 경우

==> 패턴을 따옴표(")나 작은 따옴표(')로 감싸주어야 한다.
```

## sort [option] [file] : 텍스트 파일의 내용을 알파벳 순서대로 정렬
```
-b : 공백을 무시한다.

-i : 프린트 가능한 문자만 비교

-r : 결과를 역으로 출력

-n : 숫자를 기준으로 정렬

-c : 파일이 정렬되어 있는지 검사

-o : 결과를 지정한 파일에 저장

-u : 필드 내에서 중복되는 값을 제거하고 출력 
    
    ex) abcd가 4줄 중복되면 한 줄만 출력시켜주는 것

-m : 복수의 입력 파일을 병합

-f : 모든 문자를 소문자로 인식
```

## uniq [option] : 중복되는 행을 필터링
```
-c : 행이 얼마나 중복되는지 계산하여 출력

-d : 중복되는 행의 내용을 한번만 출력

-D : 중복되는 모든 행의 내용을 출력

-N : 첫 행부터 N행까지는 검사 안함

-u : 중복되지 않는 행만 출력

무작위로 있으면 인식 x
==> 그래서 sort와 uniq 명령어가 보통은 같이 쓰임.
ex) cat data.txt | sort | uniq –u
```

## file [option] [file name] : 파일의 종류와 파일 정보 출력
```
-b : 지정한 파일명은 출력하지 않고 파일의 유형만 출력

-z : 압축된 파일 내용을 출력 
```

## strings [option] [file name] : 오브젝트 또는 이진 파일에서 인쇄 가능한 문자열 출력(ASCII 문자를 출력)
``` 
-a : 전체 파일 검색

-f : 각 문자열 이전에 파일명 출력

-min-len (-n min-len) : 최소 문자열 길이 지정, 기본 값은 4 , 즉 4줄 이상의 문자열부터 출력하는거임.
```

## xxd [option] [file name] : 리눅스 shell 상에서 binary파일의 hexdump를 보여주는 명령어
```
-b : dump가 2진수로 출력

-c [개수] : 행 당 출력되는 열 개수 설정

ex) 4byte 씩 100의 길이만큼 출력 ==> xxd –g 4 –l 100 [filename]

-g [개수] : 출력시 group으로 묶이는 byte의 개수 설정

ex) 8열로 마지막 80byte만 출력 ==> xxd –c 8 –s –80 [filename]

-l [길이] : 설정된 길이 byte만큼 출력

-p : 주소나 ASCII 없이 hexdump 내용만 출력

-u : hex를 소문자 대신 대문자로 출력

-s [+][-] 위치 : 설정된 위치에서부터 hexdump함. 
                 +위치는 파일의 시작에서부터, -위치는 파일의 끝에서 부터임. 

-i : C언어에서 사용할 수 있는 형식으로 출력

-r : 반대로 hexdump를 binary파일로 바꾸어 출력
```

## base64 [option] [file name] 
```
실행파일이나 zip파일 등을 ASCII 영역의 문자들로만 이루어진 일련의 문자열로 바꾸는 
인코딩 방식, 문자열을 base64로 인코드 디코드 해줌

-d : 디코드 시켜주고 출력, 이 옵션이 없으면 그냥 인코드 시켜주고 출력

-i : 디코딩 할 때, 알파벳 아닌 문자 무시하고 출력
```

## tr [option] string : 특정 문자를 삭제 혹은 변환한다.
```
ex) cat data.txt | tr ‘a-z’ ‘A-Z’ : data.txt를 출력할 때 소문자를 대문자로 변경

-d : 문자열 에서 지정한 문자를 삭제 후 출력

ex) cat data.txt | tr –d ‘0-9’ : data.txt를 출력할 때 숫자 삭제

-s : 문자열 에서 반복되는 문자 삭제

-s 옵션으로 반복되는 문자열 없을 때 원하는 문자로 대체 가능

ex) data.txt에서 공백이 하나밖에 없는 상황(반복 안되는 상황)

==> cat data.txt | tr –s ‘ ’ ‘@’ : data.txt.를 출력할 때 공백을 @로 대체
```

## bzip2 [option] [file name] , 확장자는 .bz2
```
-c : 압축되거나 압축을 푼 파일을 표준출력한다.

-z : 파일을 압축 , 압축 시 file name.bz2 파일 생성됨.

-d : 압축해제 

bzcat : 압축 파일 내용 보기 명령어임. (옵션아님)
```

## gzip [option] [file name] , 확장자는 .gz
```
-d : 압축 해제

-r : 개별적으로 압축 

-rd : 개별적으로 압축 해제
```

## tar [option] [file name1] [file name2] ...: 여러 파일을 묶거나 해제함
```
파일명1은 결과 파일명, 파일명2는 압축 또는 묶음 파일 

사실 압축프로그램이 아니라 백업용 프로그램임, 용량이 줄어들지 않기 때문임.

-cf archive.tar foo bar :foo와 bar 파일에서 아카이브 파일 생성

-tvf archive.tar  # 아카이브 파일에 있는 내용 모두 출력

–xf archive.tar  # 아카이브 파일 해제 

-cvf [합칠파일] [합칠파일들] : ‘합칠파일‘들을 ’합칠파일‘에다가 합쳐라.

-xvf 해제할 파일 : 해제하기

-c : 새로운 파일을 만드는 옵션

-x (extract) : 압축을 해제

-v (view) : 압축이 되거나 풀리는 과정을 출력

-f (file) : 파일로서 백업을 하겠다는 옵션

tar로 파일들을 묶은 뒤 gzip으로 압축하고 싶으면

FTZ trainer7 참고) ex) gzip songs.tar // 용량이 줄어듦.

해제할 땐 gzip을 먼저 해제하고 tar을 해제함.
```

## ssh [option] [IP] 
```
usage : ssh <host address> <option>
        ssh <name>@<host address> -> 사용자 ID 추가
```
```       
options :

-1 : ssh를 프로토콜 버전 1로 시도

-2 : ssh를 프로토콜 버전 2로 시도

-4 : IPv4 주소만 사용

-6 : IPv6 주소만 사용

-F configfile : 사용자 설정 파일(configfile)을 지정한다.

-I smartcard_device 
   :사용자 개인 RSA 키를 저장할 디바이스(smartcard_device)를 지정한다.

-i identity_file : RSA 나 DSA 인증 파일(indentity_file)을 지정한다.
 ex) ssh –i sshkey.private bandit14@localhost

-l [login_name] [IP] : 서버에 로그인할 사용자(login_name)를 지정한다.
== name@IP

-p port : 서버에 접속할 포트를 지정한다.

-q : 메시지를 출력하지 않는다.

-V : 버전 정보를 출력한다.

-v : 상세한 정보를 출력한다. 디버깅에 유용하다.

-t : 터미널 할당을 강제한다. 

원격에서 ssh를 이용한 명령어 실행 방법은 뒤에다 “명령어” 해주면 됨.
```

## telnet [option] [host [port]]
```
host : 접속할 호스트로 인터넷 주소형식으로 사용

port : 접속에 사용할 호스트의 포트를 지정, 초기값은 23번 사용

-l 사용자 ID : 텔넷서버 시스템에 접속할 사용자 ID을 지정한다.

-a : 현재 사용자를 ID로 사용하여 접속한다.

● s_client : openssl 명령으로 운영중인 웹서버의 SSL인증서 정보를 살펴볼 수 있다 

Usage : openssl s_client –connect [host]:[port] [option]

option :

-connect host:port : 접속할 호스트와 포트, 기본값은 localhost:4433

-ssl2,ssl3,tls1,dtls1 : 설정한 프로토콜만 통신

-msg : 프로토콜 메시지 출력

-cert [file] : client public key 서버인증서

-key [file] : client private key 개인키 사용 기본 perm

-pass arg : private key를 위한 password을 전달
```

## nmap: Network exploration tool and security / port scanner
```
Usage : nmap [scan type] [option] [host]

옵션에 따라 스캔된 호스트의 추가 정보를 출력하는데 
그 정보 중 핵심은 'interesting ports table'입니다. 

테이블은 스캔된 포트번호, 프로토콜, 서비스이름, 상태(state)를 출력합니다. 

state에는 open, filtered, closed, unfiltered가 있습니다. 

스캔 결과 출력 시 하나의 상태가 아닌 
조합된 값(open|filtered, closed|filtered)을 출력할 수도 있습니다.

open: 스캔된 포트가 listen 상태임을 나타냄
filtered: 방화벽이나 필터에 막혀 해당 포트의 open, close 여부를 알 수 없을 때
closed: 포트스캔을 한 시점에는 listen 상태가 아님을 나타냄
unfiltered: 스캔에 응답은 하지만 해당 포트의 open, close 여부는 알 수 없을 때

옵션 중 –p <범위> 를 이용해 지정한 범위 내 포트를 스캔한다.
```

## nc [option] [hostname] [port] : TCP ,UDP 프로토콜을 사용하는 네트워크 연결에서 데이터를 읽고 쓰는 명령어.
```
네트워크 연결상태를 읽거나 쓸 때 사용됨.

option

-n : 호스트 네임과 포트를 숫자로만 입력받는다.

-v : 더 많은 정보를 얻을 수 있다.

-u : TCP 연결대신 UDP 연결이 이루어 진다.

-p : local-port를 지정한다. 주로 –l와 같이 사용.

-l : listen 모드로 nc를 띄우게 된다. nc를 서버로 이용 시 사용되므로 
     대상 호스트는 입력하지 않음.

-r :여러개의 포트를 지정했을 때 스캐닝 순서를 랜덤하게 한다.

-z : 연결을 이루기위한 최소한의 데이터 외에는 보내지 않게 하는 옵션

-e [program] : 연결되었을 때, 프로그램을 실행함 (위험) 
  ex) : nc –l –p [port] –e  /bin/bash // 리눅스 환경에서 백도어 쉘
  ex) : nc –l –p [port] -e cmd.exe // 윈도우 환경에서 백도어 쉘
```