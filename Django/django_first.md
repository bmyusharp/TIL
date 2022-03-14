# 가상환경

> 관리의 용이함을 위해, 패키지 등 확장 프로그램들을 개별적으로 적용하기 위해 가상환경을 만듭니다.



가상환경 만들기

```
$ python -m venv venv
```

가상환경으로 이동

```
$ source venv/Scripts/activate
```

(venv) 확인



**자동완성** 기능

첫자 누르고 tab

**프로젝트 이름에는 파이썬이나 쟝고에서 사용중인 키워드를 사용할 수 없다. 하이픈(-)도 사용할 수 없다. (e.g. Django, text, class, django-text 등)**



Django 설치

```bash
$ pip install django==3.2.12
```

만약 4.x버전 설치한 경우 pip uninstall django 후 재설치



서버 켜기(on)

```
$ python manage.py runserver
```

끄기: Ctrl + C









# 프로젝트 생성

> 가상환경 - 쟝고 설치 - 프로젝트 생성

```bash
$ django-admin startproject pjt .
```

pjt (프로젝트명)



## 프로젝트 구조

볼드체 된 두 파일만 주로 작업할 예정임.

`__init__.py`: *Python에게 이 디렉토리를 하나의 Python 패키지로 다루도록 지시, 건드릴 일이 없음!*

`asgi.py`: *Asychronous Server Gateway Interface. 웹 서버와 연결 및 소통하는 것을 도움 (배포). 이것도 당분간은 사용하진 않게 될 것*

`settings.py`: **애플리케이션의 모든 설정을 포함.**

`urls.py`: **사이트의 url과 적절한 views의 연결을 지정**

`wsgi.py`: *Web Server Gateway Interface. 이것도 당분간 사용하지 않음*

`manage.py`: *Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인 유틸리티. 많은 명령어들이 등장하게 되는데, 이 파일을 통해 command를 동작시킬 것 (runserver ...) 이것도 직접적으로 터치하지 않음.*



### settings.py 파일 설정

LANGUAGE_CODE 부터 USE_TZ 까지의 5줄 설정 해주기

![image-20220303092426488](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220303092426488.png)









# Application 생성 (MTV)

> 프로젝트 생성 - settings.py 설정 - 앱 생성 - 앱 등록(settings)

일반적으로 복수형으로 하는 것을 권장

```
$ python manage.py startapp articles 
```

articles (앱 이름)

**앱을 만들고 나면, settings.py에서 앱 등록하기**

![Image Pasted at 2022-3-3 09-27](C:\Users\1004r\Downloads\Image Pasted at 2022-3-3 09-27.png)





## 앱 내부의 파일 구조

`admin.py`: **관리자용 페이지를 설정 하는 곳**

`apps.py`: *앱의 정보가 작성된 곳 (수정 X)*

`models.py`: **앱에서 사용하는 Model을 정의하는 곳** MTV의 M

`test.py`: *프로젝트의 테스트 코드를 작성하는 곳 (얘도 사용 X)*

`view.py`: **view 함수들의 정의 되는 곳.** MTV의 V

그럼 Template은? 우리가 **직접 생성**해줘야 함.

`migrations`폴더: models.py와 관련이 있음



### urls.py

> path('<함수명>/', views.<함수명>),

```python
from articles import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index), 
    #views의 index함수를 실행하겠다.
]
```

**urls.py 상단에 articles에서 views를 가져오는 걸 잊지 말자**

url은 하나의 view 함수만 매핑 가능하다!

url을 마쳤으니 view로 넘어가자.



### view.py

> def <함수명>(request):
>
> ​	lunch_box = ['햄버거', '피자', '김밥']
>
> ​	lunch_menu = random.choice(lunch_box)
>
> ​	context = {
>
> ​		'lunch_box': lunch_box,
>
> ​		'lunch_menu': lunch_menu
>
> }

> ​	return render(request, '<함수명>.html', context)

```python
from django.shortcuts import render

# Create your views here.
def index(request): 
    #view함수가 무조건 받아야 하는 인자임. (request) 안받을 시 에러
    return render(request, 'index.html') 
	#두번째 인자로 템플릿을 받음
```



그 다음에 Templete폴더를 articles 내부에 만들고 Template 폴더 내부에 lunch.html 생성 후 작성

```html
<h1>점심메뉴 추천</h1>
<h2>{{ lunch_menu }} 이거 먹어!</h2>
```

![image-20220303095753085](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220303095753085.png)

전체적 과정



### settings.py

```python
...
# 107 line
LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'
```



# https://www.djangoproject.com/

## Django의 문법

{{  }}: 출력

{% %}: 문법 (% 사이에)

{% for %} 사용 시 {% endfor %}로 닫음 if도 마찬가지





# 기타

## .gitignore

- git은 해당 저장소가 있는 폴더의 모든 파일의 변경 사항을 추적한다.
  - 폴더 중에서 특정 파일/폴더/확장자는 git으로 관리하고 싶지 않다.
  - 개별적으로 선택해서 git으로 작업하는것도 되지만, 이럴 때는 편하게 gitignore를 사용한다.
- 일반적으로 OS, 특정 언어, 개발 환경(IDE), 특정 프레임워크에서 버전 관리에 사용하지 않는 파일들이 있다.
  - https://gitignore.io/

```python
# Mac
.DS_Store

# Python 가상환경
venv/

# Pycharm
.idea/

# Visual Studio Code
.vscode/
```

