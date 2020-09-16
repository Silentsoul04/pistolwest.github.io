---
title : "Python module 정리"
excerpt : "쓰는 module 정리"
toc : true
toc_sticky : true
categories :
  - Crypto
  - Python
tags :
  - module 정리
---

## 1. Crypto
Crypto.Cipher, Crypto.Hash, Crypto.Random 등등이 있는데 자세한 건 여기서 확인 가능 <a href="https://pycryptodome.readthedocs.io/en/latest/src/util/util.html#module-Crypto.Util.number" target="_blank">Here</a>  
```python
# Crypto.Util.number module
from Crypto.Util.number import *

Crypto.Util.number.GCD(x,y) # x,y의 최대공약수

Crypto.Util.number.bytes_to_long(s) # byte string을 long int(big endian)으로 변경
# -> python 3.2 이상에서는 내장 메소드 사용 ex) int.from_bytes(b'P','big')

Crypto.Util.number.inverse(u,v) # u mod(v)의 역원을 리턴

Crypto.Util.number.long_to_bytes(n, blocksize=0) # long int를 byte string으로 변환
# -> 내장 메소드 사용 ex) n=80; n.to_bytes(2,'big')
```

## 2. re
정규 표현식을 지원하기 위한 모듈
```  
자주 사용하는 문자 클래스

\d - 숫자와 매치, [0-9]와 동일한 표현식이다.

\D - 숫자가 아닌 것과 매치, [^0-9]와 동일한 표현식이다.

\s - whitespace 문자와 매치, [ \t\n\r\f\v]와 동일한 표현식이다. 
     맨 앞의 빈 칸은 공백문자(space)를 의미한다.

\S - whitespace 문자가 아닌 것과 매치, [^ \t\n\r\f\v]와 동일한 표현식이다.

\w - 문자+숫자(alphanumeric)와 매치, [a-zA-Z0-9_]와 동일한 표현식이다.

\W - 문자+숫자(alphanumeric)가 아닌 문자와 매치, [^a-zA-Z0-9_]와 동일한 표현식이다.
```
```python
import re

re.search(pattern, string) 
# string에서 pattern과 일치하는 첫 번째 위치를 찾음.

re.findall(pattern, string) 
# 정규식과 매치되는 모든 문자열을 리스트로 돌려준다. 
# ex) re.findall("\d+",msg)
```

## 3. Image module in PIL(pillow) 
파이썬 이미징 라이브러리로 다양한 이미지 처리 기능 제공.  
자세한건 여기 <a href="https://pillow.readthedocs.io/en/stable/reference/Image.html#PIL.Image.Image.convert" target="_blank">Here</a>   
참고 : <a href="https://2nit.tistory.com/22" target="_blank">https://2nit.tistory.com/22 [Inha init]</a>  
       <a href="http://pythonstudy.xyz/python/article/406-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%B2%98%EB%A6%AC-Pillow" target="_blank">http://pythonstudy.xyz/python/article/406-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%B2%98%EB%A6%AC-Pillow</a>

```python
from PIL import Image

# 이미지 열기
im = Image.open('python.png')

#새로운 이미지 파일 생성 Image.new(mode, size, color)
new = Image.new('new.png') 
im2 = Image.new("RGB", (500,500), (200,200,200)) # color default : black(0,0,0)
im3 = Image.new("RGB", (200,200))  

## mode 종류
'''
1 (1-bit pixels, black and white, stored with one pixel per byte)
L (8-bit pixels, black and white)
P (8-bit pixels, mapped to any other mode using a color palette)
RGB (3x8-bit pixels, true color)
RGBA (4x8-bit pixels, true color with transparency mask)
CMYK (4x8-bit pixels, color separation)
YCbCr (3x8-bit pixels, color video format)
LAB (3x8-bit pixels, the L*a*b color space)
HSV (3x8-bit pixels, Hue, Saturation, Value color space)
I (32-bit signed integer pixels)
F (32-bit floating point pixels)
'''

# 이미지 붙이기
im2.paste(im3, (20,20,220,220)) # im2에 im3 붙이기
# 2개 튜플인 경우 -> 왼쪽 상단에서 시작, (left, upper)
# 4개 튜플인 경우 -> 왼쪽 상단에서 시작, (left, upper, right, lower)
# 4개인 경우 반드시 덧씌울 공간의 값이 덧씌우는 이미지보다 크면 안됌.
im2.save("paste_result.jpg")

# 이미지의 각 픽셀에 접근
pix=im.load()

# 이미지 크기 출력
print(im.size) # return (width, height)
 
# 이미지 JPG로 저장
im.save('python.jpg')

# 이미지 포맷 출력
print(im.format)

# Thumbnail 이미지 생성
size = (64, 64)
im.thumbnail(size)

# 이미지 cropping(일부 잘라내기)
cropImage = im.crop((100, 100, 150, 150)) #(좌, 상, 우, 하)
cropImage.save('python-crop.jpg')

# 이미지 copy
im2 = im.copy()

# 이미지 회전 및 Resize
img2 = im.resize((600,600)) # 크기를 600x600으로

img3 = im.rotate(90) # 90도 시계방향 회전

# 이미지 필터링 -> 필터종류는 ImageFilter 모듈 import 하여 지정

from PIL import Image, ImageFilter
 
im = Image.open('python.png')
blurImage = im.filter(ImageFilter.BLUR)
 
blurImage.save('python-blur.png')
```
```
필터 종류는 다음과 같음
BLUR 
CONTOUR
DETAIL
EDGE_ENHANCE
EDGE_ENHANCE_MORE
EMBOSS
FIND_EDGES
SHARPEN
SMOOTH
SMOOTH_MORE
```

### 3-1. PIL을 이용한 CTF 문제 풀이
문제 : <a href="https://pistolwest.github.io/crypto/houseplant-crypto/#6-rainbow-vomit" target="_blank">HouseplantCTF Rainbow Vomit</a>
```python
from PIL import Image

'''
taking input along lines
'''

# striping surrounding white pixels

'''
im=Image.open('output.png')

pixels=im.load()

im1=Image.new('RGB',(100,30))
pixy=im1.load()

for i in range(2,32):
	for j in range(2,102):
		pixy[j-2,i-2]=pixels[j,i]

im1.save('better.png')
'''

colour={"B":(0,0,0),
"W":(255,255,255),
"G":(128,128,128),
"r":(255,0,0),
"g":(0,255,0),
"b":(0,0,255),
"y":(255,255,0),
"c":(0,255,255),
"m":(255,0,255)
}

a=[colour["m"],colour["r"],colour["g"],colour["y"],colour["b"],colour["c"]]
b=[colour["r"],colour["m"],colour["g"],colour["y"],colour["b"],colour["c"]]
c=[colour["r"],colour["g"],colour["m"],colour["y"],colour["b"],colour["c"]]
d=[colour["r"],colour["g"],colour["y"],colour["m"],colour["b"],colour["c"]]
e=[colour["r"],colour["g"],colour["y"],colour["b"],colour["m"],colour["c"]]
f=[colour["r"],colour["g"],colour["y"],colour["b"],colour["c"],colour["m"]]
g=[colour["g"],colour["r"],colour["y"],colour["b"],colour["c"],colour["m"]]
h=[colour["g"],colour["y"],colour["r"],colour["b"],colour["c"],colour["m"]]
i=[colour["g"],colour["y"],colour["b"],colour["r"],colour["c"],colour["m"]]
j=[colour["g"],colour["y"],colour["b"],colour["c"],colour["r"],colour["m"]]
k=[colour["g"],colour["y"],colour["b"],colour["c"],colour["m"],colour["r"]]
l=[colour["y"],colour["g"],colour["b"],colour["c"],colour["m"],colour["r"]]
m=[colour["y"],colour["b"],colour["g"],colour["c"],colour["m"],colour["r"]]
n=[colour["y"],colour["b"],colour["c"],colour["g"],colour["m"],colour["r"]]
o=[colour["y"],colour["b"],colour["c"],colour["m"],colour["g"],colour["r"]]
p=[colour["y"],colour["b"],colour["c"],colour["m"],colour["r"],colour["g"]]
q=[colour["b"],colour["y"],colour["c"],colour["m"],colour["r"],colour["g"]]
r=[colour["b"],colour["c"],colour["y"],colour["m"],colour["r"],colour["g"]]
s=[colour["b"],colour["c"],colour["m"],colour["y"],colour["r"],colour["g"]]
t=[colour["b"],colour["c"],colour["m"],colour["r"],colour["y"],colour["g"]]
u=[colour["b"],colour["c"],colour["m"],colour["r"],colour["g"],colour["y"]]
v=[colour["c"],colour["b"],colour["m"],colour["r"],colour["g"],colour["y"]]
w=[colour["c"],colour["m"],colour["b"],colour["r"],colour["g"],colour["y"]]
x=[colour["c"],colour["m"],colour["r"],colour["b"],colour["g"],colour["y"]]
y=[colour["c"],colour["m"],colour["r"],colour["g"],colour["b"],colour["y"]]
z=[colour["c"],colour["m"],colour["r"],colour["g"],colour["y"],colour["b"]]

A0=[colour["B"],colour["G"],colour["W"],colour["B"],colour["G"],colour["W"]]
A1=[colour["G"],colour["B"],colour["W"],colour["B"],colour["G"],colour["W"]]
A2=[colour["G"],colour["W"],colour["B"],colour["B"],colour["G"],colour["W"]]
A3=[colour["G"],colour["W"],colour["B"],colour["G"],colour["B"],colour["W"]]
A4=[colour["G"],colour["W"],colour["B"],colour["G"],colour["W"],colour["B"]]
A5=[colour["W"],colour["G"],colour["B"],colour["G"],colour["W"],colour["B"]]
A6=[colour["W"],colour["B"],colour["G"],colour["G"],colour["W"],colour["B"]]
A7=[colour["W"],colour["B"],colour["G"],colour["W"],colour["G"],colour["B"]]
A8=[colour["W"],colour["B"],colour["G"],colour["W"],colour["B"],colour["G"]]
A9=[colour["B"],colour["W"],colour["G"],colour["W"],colour["B"],colour["G"]]
space=[colour["W"],colour["W"],colour["W"],colour["W"],colour["W"],colour["W"]]
space2=[colour["B"],colour["B"],colour["B"],colour["B"],colour["B"],colour["B"]]
comma=[colour["W"],colour["B"],colour["B"],colour["W"],colour["W"],colour["B"]]
period=[colour["B"],colour["W"],colour["W"],colour["B"],colour["B"],colour["W"]]

letters={'a':a,'b':b,'c':c,'d':d,'e':e,'f':f,'g':g,'h':h,'i':i,'j':j,'k':k,'l':l,'m':m,'n':n,'o':o,'p':p,'q':q,'r':r,'s':s,'t':t,'u':u,'v':v,'w':w,'x':x,'y':y,'z':z,'0':A0,'1':A1,'2':A2,'3':A3,'4':A4,'5':A5,'6':A6,'7':A7,'8':A8,'9':A9,' ':space,',':comma,'.':period}


im=Image.open("better.png")
pixels=im.load()

#print(im.size,im.height)

#main code... 
hexhue=[]

for I in range(0,im.height,3):
	for J in range(0,im.width,2):
		partpix=[]
		for K in range(3):
			for L in range(2):
				partpix.append(pixels[J+L,I+K])

		hexhue.append(partpix)

#print(hexhue)
plaintext=''
for A in hexhue:
	for B in letters:
		if letters[B]==A:
			plaintext+=B

print(plaintext)
```