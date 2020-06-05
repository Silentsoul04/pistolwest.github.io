---
title : "Django 작성해야 할 python code들"
excerpt : "Django 작성해야 할 python code들"
toc : true
toc_sticky : true
categories :
  - Python
tags :
  - Django
---

## 0. 참고자료 출처 및 주의사항
<a href="https://ssungkang.tistory.com/entry/Django-02-Django-%EC%8B%9C%EC%9E%91-Hello-World-%EC%B6%9C%EB%A0%A5?category=320582" target="_blank">참고</a>   
아래 2번부터는 정해진 순서는 없고 해보니까 이렇게 하는게 편하다는 것.  
보고 까먹은건 참고에서 다시 확인  

## 1. models.py
models.py에는 내가 만들 앱에 적용시킬 class를 만드는 곳.   
models를 db에 연결  
```
python3 manage.py makemigrations --> 앱 디렉토리 밑에 migrations 폴더를 생성하여 db와 소통해준다.   
-> python3 manage.py migrate --> db에 최신 models (변경사항) 적용 -> 앱 디렉토리에 있는 admin.py에 등록.
```    
ex) 블로그 생성을 위한 blog 객체 생성
```python
from django.db import models

class Blog(models.Model):
	title = models.CharField(max_length = 200)
	writer = models.CharField(max_length = 30, default="anonymous", null=True) 
    # title과 field가 곂쳐서 오류가 나서 수정한 부분. 오류 내용은 까먹음
	pub_date = models.DateField('date published')
	body = models.TextField()
	def __str__(self) :
		return self.title
	def summary(self) :
		return self.body[:100]
```

## 2. views.py
views.py에는 구체적인 함수들을 작성  
ex) blog에서 구현할 login, logout, register, modify 등등
```python
from django.shortcuts import render, get_object_or_404, redirect # render 제외 2개 추가
from django.utils import timezone
from .models import Blog    ## models.py에 있는 Blog 객체 사용
from django.contrib.auth.models import User # login, logout, register를 위해 추가
from django.contrib import auth # login, logout, register를 위해 추가 

def login(request) :
	if request.method == 'POST' :
		user_id = request.POST['user_id']
		user_pw = request.POST['user_pw']
		user = auth.authenticate(request, username=user_id, password=user_pw)	
		if user is not None :
			auth.login(request, user)	
			return redirect('home')
		else :
			return render(request, 'login.html', {'error': 'ID or PW is incorrect'})
	else :
		return render(request,'login.html')
	
def register(request) :
	if request.method == "POST" :
		if request.POST['user_pw1'] == request.POST['user_pw2'] :
			user = User.objects.create_user( username=request.POST['user_id'], password=request.POST['user_pw1'],
		        				 email=request.POST['user_email'])
			auth.login(request,user)
			return redirect('home')
		else :
			return render(request, 'register.html', {'error' : 'Confirm PW'})	
	return render(request, 'register.html')
	
def logout(request) :
	auth.logout(request)
	return render(request, 'login.html')
	
def home(request):
	blogs = Blog.objects
	return render(request, 'home.html', {'blogs' : blogs})

def detail(request, blog_id):
	blog_detail = get_object_or_404(Blog, pk=blog_id)
	return render(request, 'detail.html', {'blog' : blog_detail})

def write(request) :
	return render(request, 'write.html')

def create(request) :
	blog = Blog()
	blog.title = request.GET['title']
	blog.writer = request.user.get_username()
	blog.body = request.GET['body']
	blog.pub_date = timezone.datetime.now()
	blog.save()
	return redirect('home')

def modify(request, blog_id):
	blog = get_object_or_404(Blog, pk=blog_id)
	if request.method == "POST" :
		blog.title= request.POST['title']
		blog.body = request.POST['body']
		blog.pub_date= timezone.datetime.now()
		blog.save()
		return redirect('/blog/'+str(blog.id)) 
	else :
		return render(request, 'modify.html', {'blog' : blog}) 
		
def delete(request, blog_id):
	blog = Blog.objects.get(pk=blog_id)
	blog.delete()
	return redirect('home')	
``` 

## 3. urls.py
```
url 설계 -> 해당 url로 접근 시, views.py에 있는 함수를 실행시킴 
   -> 즉, 그 함수가 html 파일을 화면에 보여주는 원리 
   --> 앱 폴더에서 urls.py를 작성 --> 그 내용을 프로젝트 폴더에 있는 urls.py에도 작성
```

### 3-1. 프로젝트 폴더 안에 있는 urls.py
```python
"""work URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/2.2/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
	path('admin/', admin.site.urls) 
	path('앱이름/', include('앱이름.urls')), 
]
```
위에 예제들이 있고 그 중 3번째를 사용


### 3-2. 앱 안에 있는 urls.py
```python
from django.urls import path
import 앱이름.views  
# 각각의 app에 만든 하위 urls.py는 path에서 VIEW의 정보를 argument로 받기 때문에, 반드시 views.py를 import 해야한다.

urlpatterns = [
	# path('admin/', admin.site.urls)  
    # --> 기본 urls.py에만 있으면 됨. 아니면 자꾸 오류 메세지 출력 
	path('',앱이름.views.함수, name='index'),  
    #경로가 ''이면 접속 시 기본적으로 index.html을 불러오는 것. 
]
```
```python
Example code

from django.urls import path
import blog.views

urlpatterns = [
	path('', blog.views.login, name = 'login'), 
    # 기본 페이지가 login 페이지, views.py에 있는 login 함수를 통해 login.html 페이지를 불러옴.
	path('blog/register', blog.views.register, name = 'register'),
	path('blog/board', blog.views.home, name='home'),
	path('blog/<int:blog_id>', blog.views.detail, name = 'detail'),
	path('blog/write', blog.views.write, name = 'write'),
	path('blog/create', blog.views.create, name = 'create'),
	path('blog/<int:blog_id>/modify', blog.views.modify, name = 'modify'),
	path('blog/<int:blog_id>/delete', blog.views.delete, name = 'delete'),
	path('blog/logout', blog.views.logout, name = 'logout'),
]
```
## 4. 웹 페이지 상에 보여줄 html 파일 작성
앱 폴더 안에 templates 폴더 생성 후 (앱과 동일한 이름 폴더 생성) html 파일 작성 (views.py에 사용될 html파일)  
ex) login.html, detail.html, register.html 등등..  

### 4-1. 템플릿 언어
<a href="https://ssungkang.tistory.com/entry/Django-%ED%85%9C%ED%94%8C%EB%A6%BF-%EC%96%B8%EC%96%B4%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90?category=320582" target="_blank">여기서 템플릿 언어라는 것이 사용됨.</a>   
템플릿 언어를 통해 더 많은 기능을 구현 가능  

### 4-2. 쿼리셋과 메소드
<a href="https://ssungkang.tistory.com/entry/Django-05-queryset-%EA%B3%BC-method?category=320582" target="_blank">참고</a>  

## 5. admin.py
```python
from django.contrib import admin
from .models import 클래스 이름

admin.site.register(클래스이름)

''' ex)
from django.contrib import admin
from .models import Blog

admin.site.register(Blog)
'''
```
admin 계정 만들어서 관리 가능. python3 manage.py createsuperuser
