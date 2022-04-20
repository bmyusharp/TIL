[TOC]

# 1. HTTP

- HyperText Transfer Protocol
- 웹 상에서 컨텐츠를 전송하기 위한 약속
- HTML 문서와 같은 리소스들을 가져올 수 있도록 하는 프로토콜(규칙, 약속)
- 웹에서 이루어지는 모든 데이터 교환의 기초
  - 요청(request)
    - 클라이언트에 의해 전송되는 메시지
  - 응답(response)
    - 서버에서 응답으로 전송되는 메시지
- 기본 특성
  - Stateless
  - Connectionless
- 쿠키와 세션을 통해 서버 상태를 요청과 연결하도록 함

![image-20220420090617912](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220420090617912.png)

## 1.1. HTTP request methods

- 자원에 대한 행위(수행하고자 하는 동작)을 정의
- 주어진 리소스(자원)에 수행하길 원하는 행동을 나타냄
- HTTP Method 예시
  - GET, POST, PUT, DELETE
- 특정 HTTP 요청이 성공적으로 완료되었는지 여부를 나타냄
- 응답은 5개의 그룹으로 나뉘어짐
  1. Informational responses (1xx)
  2. Successful responses (2xx)
  3. Redirection messages (3xx)
  4. Client error responses (4xx)
  5. Server error responses (5xx)

## 1.2. 웹에서의 리소스 식별

- HTTP 요청의 대상을 리소스 (resource, 자원) 라고 함
- 리소스는 문서, 사진 또는 기타 어떤 것이든 될 수 있음
- 각 리소스는 리소스 식별을 위해 HTTP 전체에서 사용되는 URI(Uniform Resource Identifier)로 식별됨

## 1.3. URL, URN

- **URL (Uniform Resource Locator)**
  - 통합 자원 위치
  - 네트워크 상에 자원이 어디 있는지 알려주기 위한 약속
  - 과거에는 실제 자원의 위치를 나타냈지만 현재는 추상화된 의미론적인 구성
  - '웹 주소', '링크'라고도 불림
- **URN (Uniform Resource Name)**
  - 통합 자원 이름
  - URL과 달리 자원의 위치에 영향을 받지않는 유일한 이름 역할을 함
  - 예시
    - ISBM (국제표준도서번호)

## 1.4. URI

- Uniform Resource Identifier
  - 통합 자원 식별자
  - 인터넷의 자원을 식별하는 유일한 주소 (정보의 자원을 표현)
  - 인터넷에서 자원을 식별하거나 이름을 지정하는데 사용되는 간단한 문자열
  - 하위 개념
    - URL, URN
  - URI는 크게 URL과 URN으로 나눌 수 있지만, URN을 사용하는 비중이 매우 적기 때문에 일반적으로 URL은 URI와 같은 의미처럼 사용하기도 함.
  - 인터넷에서 정보를 어떻게 표현할 것인가...

## 1.5.URI의 구조

- **Scheme (protocol)**

  - 브라우저가 사용해야 하는 프로토콜
  - http(s), data, file, ftp, mailto

- **Host (Domain name)**

  - 요청을 받는 웹 서버의 이름

  - IP address를 직접 사용할 수도 있지만, 실 사용시 불편하므로 웹에서 그리 자주 사용되지는 않음

    (google의 IP address - 142.251.42.142)

  - www.example.com

- **Port**

  - 웹 서버 상의 리소스에 접근하는데 사용되는 기술적인 '문 (gate)'
  - HTTP 프로토콜의 표준 포트
    - HTTP 80
    - HTTPS 443
  - *.com*`:80`

- **Path**
  - 웹 서버 상의 리소스 경로
  - 초기에는 실제 파일이 위치한 물리적 위치를 나타냈지만, 오늘날은 물리적인 실제 위치가 아닌 추상화 형태의 구조로 표현
  - `/path/to/myfile.html`/

- **Query (Identifier)**
  - Query String Parameters (GET 배울 때 많이 사용함)
  - 웹 서버에 제공되는 추가적인 매개 변수
  - & 로 구분되는 key-value 목록
  - `?key=value`
- **Fragment**
  - Anchor
  - 자원 안에서의 북마크의 한 종류를 나타냄
  - 브라우저에게 해당 문서 (HTML) 의 특정 부분을 보여주기 위한 방법
  - 브라우저에게 알려주는 요소이기 때문에 fragment identifier (부분 식별자) 라고 부르며 '#' 뒤의 부분은 요청이 서버에 보내지지 않음
  - `#quick-start`



# 2. RESTful API

## 2.1. API

- Application Programming Interface
- 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스
  - 애플리케이션과 프로그래밍으로 소통하는 방법
  - CLI는 명령줄, GUI는 그래픽 (아이콘), API는 프로그래밍을 통해 특정한 기능 수행
- Web API
  - **웹 애플리케이션 개발**에서 다른 서비스에 요청을 보내고 응답을 받기 위해 **정의된 명세**
  - 현재 웹 개발은 모든 것을 직접 개발하기보다 여러 Open API를 활용하는 추세
- 응답 데이터 타입
  - HTML, XML, JSON 등

- 대표적인 API 서비스 목록
  - Youtube API, Naver Papago API, Kakao Map API...

## 2.2. REST

- **RE**presentational **S**tate **T**ransfer
- API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론
  - 2000년 로이 필딩의 박사학위 논문에서 처음으로 소개 된 후 네트워킹 문화에 널리 퍼짐
- 네트워크 구조 (Network Architecture) 원리의 모음
  - 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법
- REST 원리를 따르는 시스템을 RESTful 이란 용어로 지칭함 (REST 하다 라는 뜻)



- 자원을 정의하는 방법에 대한 고민
  - ex) 정의된 자원을 어디에 위치 시킬 것인가
- REST의 자원과 주소의 지정 방법
  1. 자원
     - URI
  2. 행위
     - HTTP Method
  3. **표현** (여태까지는 html-페이지-로만 하고 있었음)
     - 자원과 행위를 통해 궁극적으로 표현되는 (추상화된) 결과물
     - **JSON**으로 표현된 데이터를 제공

- REST의 핵심 규칙
  1. '정보'는 URI로 표현
  2. 자원에 대한 '행위'는 HTTP Method로 표현 (GET, POST, PUT, DELETE)
- 설계 방법론은 지키지 않았을 때 잃는 것보다 **지켰을 때 얻는 것이 훨씬 많음**
  - 단, 설계 방법론을 지키지 않더라도 동작 여부에 큰 영향을 미치지는 않음

## 2.3. JSON

- JSON (JavaScript Object Notation)
  - JSON is a lightweight data-interchange format
  - JavaScript의 표기법을 따른 단순 문자열
- 특징
  - 사람이 읽거나 쓰기 쉽고 기계가 파싱(해석/분석)하고 만들어내기 쉬움
  - 파이썬의 dictionary, 자바스크립트의 object처럼 C계열의 언어가 갖고 있는 자료구조로 쉽게 변화할 수 있는 key-value 형태의 구조를 갖고 있음

## 2.4. RESTful API

- REST 원리를 따라 설계한 API
- RESTful services, 혹은 simply REST services 라고도 부름
- 프로그래밍을 통해 클라이언트의 요청에 JSON을 응답하는 서버를 구성
  - 지금까지 사용자의 입장에서 썼던 API를 제공자의 입장이 되어 개발해보기



# 3. Response

## 3.1. Init Project

- 가상환경 설정 및 패키지 설치

```bash
$ python -m venv venv
$ source venv/Scripts/activate
$ pip install -r requirements.txt
```

- 설치된 app 확인

```python
# settings.py
INSTALLED_APPS = [
    'articles',
    'django_seed',
    ...,
]
```

- 작성된 url 확인

```python
# my_api/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('articles.urls')),
]
```

```python
# articles/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('html/', views.article_html),
    path('json-1/', views.article_json_1),
    path('json-2/', views.article_json_2),
    path('json-3/', views.article_json_3),
]
```



## 3.2. Create Dummy Data

- 작성된 model 확인

```python
# models.py

from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

- django-seed 라이브러리를 사용해 모델 구조에 맞는 데이터 생성

```bash
$ python manage.py migrate
$ python manage.py seed articles --number=20
```



## 3.3. Response

### 3.3.1. HTML

- HTML을 응답하는 서버

```python
# articles/urls.py
	path('html/', views.article_html),
```

```python
# articles/views.py
from django.shortcuts import render
from .models import Article

def article_html(request):
    articles = Article.objects.all()
    context = {
        'articles': articles,
    }
    return render(request, 'articles/article.html', context)
```

```html
<!-- articles/article.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>Article List</h1>
  <hr>
  <p>
    {% for article in articles %}
      <h2>{{ article.pk }}번 글. {{ article.title }}</h2>
      <p>{{ article.content }}</p>
      <hr>
    {% endfor %}
  </p>
</body>
</html>
```

![image-20220420100820411](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220420100820411.png)



### 3.3.2. JsonResponse

- JsonResponse 객체를 활용한 JSON 데이터 응답

```python
# articles/urls.py
    path('json-1/', views.article_json_1),
```

```python
# articles/views.py
from django.http.response import JsonResponse

def article_json_1(request):
    articles = Article.objects.all()
    articles_json = [] # 리스트임.ㅈ

    for article in articles:
        articles_json.append(
            {
                'id': article.pk,
                'title': article.title,
                'content': article.content,
                'created_at': article.created_at,
                'updated_at': article.updated_at,
            }
        )
    return JsonResponse(articles_json, safe=False)
	# JsonResponse 함수 사용됨
```

- JsonResponse 객체를 활용한 JSON 데이터 응답 확인

![image-20220420102943623](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220420102943623.png)

- **Content-Type** entity header
  - 데이터의 media type (MIME type, content type) 을 나타내기 위해 사용됨
  - 응답 내에 있는 컨텐츠의 컨텐츠 유형이 실제로 무엇인지 클라이언트에게 알려줌

- **JsonResponse** objects
  - JSON-encoded response를 만드는 HttpResponse의 서브 클래스
  - "safe" parameter
    - True (기본값)
    - dict 이외의 객체를 **직렬화 (Serialization)** 하려면 False 로 설정해야 함

```python
# JsonResponse 예시

response = JsonResponse({'foo': 'bar'})
response = JsonResponse([1, 2, 3], safe=False)
```
#### 3.3.2.1. Serialization
- "직렬화"
- 데이터 구조나 객체 상태를 동일하거나 다른 컴퓨터 환경에 저장하고, 나중에 재구성할 수 있는 포맷으로 변환하는 과정
- Serializers in Django
  - Queryset 및 Model Instance와 같은 복잡한 데이터를 JSON, XML 등의 유형으로 쉽게 변환 할 수 있는 **Python 데이터 타입**으로 만들어 줌

![image-20220420102518933](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220420102518933.png)



### 3.3.3. Django Serializer

- Django의 내장 HttpResponse를 활용한 JSON 응답

```python
# articles/urls.py
	path('json-2/', views.article_json_2),
```

- Django의 내장 HttpResponse를 활용한 JSON 응답 객체
- 주어진 모델 정보를 활용하기 때문에 이전과 달리 필드를 개별적으로 직접 만들어 줄 필요 없음

```python
# articles/views.py

from django.http.response import JsonResponse, HttpResponse
from django.core import serializers

def article_json_2(request):
    articles = Article.objects.all()
    data = serializers.serialize('json', articles)
    return HttpResponse(data, content_type='application/json')
```

- Django의 내장 HttpResponse를 활용한 JSON 응답 확인

![image-20220420103002902](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220420103002902.png)

- model, pk, fields가 생기고 이전에 봤던 필드들이 fields에 들어감.



### 3.3.4. Django REST Framework

- Django REST framework(DRF) 라이브러리를 사용한 JSON 응답
- 설치 과정 확인

```bash
$ pip install djangorestframework
```

```python
# settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

- DRF 라이브러리를 사용한 JSON 응답 url 확인

```python
# articles/urls.py
    path('json-3/', views.article_json_3),
```

- Article 모델에 맞춰 자동으로 필드를 생성해 serialize 해주는 ModelSerializer 확인

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article


class ArticleSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = '__all__'
```
ArticleSerializer: 게시글 쿼리셋 - serialize 해주는 도구 (그런데 ModelForm과 유사하게 생김)

- **DRF의** Response() 를 활용해 Serialize 된 JSON 객체 응답

  (DRF를 이용한 view 함수 작성은 다음 챕터에서 진행)

```python
# articles/views.py

from rest_framework.decorators import api_view
from rest_framework.response import Response
from .serializers import ArticleSerializer

# @api_view(['GET'])
@api_view()
def article_json_3(request):
    articles = Article.objects.all()
    serializer = ArticleSerializer(articles, many=True)
    return Response(serializer.data)
```

- DRF 라이브러리를 사용한 JSON 응답 확인

![image-20220420103930047](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220420103930047.png)

아예 처음 보는 페이지... 응답이 더 많음

![image-20220420104107143](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220420104107143.png)

- python  파일을 통해 직접 요청 보낸 후 응답 확인해보기 (requests 라이브러리를 활용)

```python
import requests
from pprint import pprint

response = requests.get('http://127.0.0.1:8000/api/v1/json-3/')
pprint(response.json())
# 파싱 과정 (언어마다 다를 수 있음)
# 서버 실행 후 파일 실행(요청 보냄)
```



애초에 render할 것이 없기 때문에 templates가 한두개밖에 없을 예정임

- Web API 구축을 위한 강력한 Toolkit을 제공하는 라이브러리
- DRF의 Serializer는 Django의 Form 및 ModelForm 클래스와 매우 유사하게 구성되고 작동함
- Web API
  - 웹 애플리케이션 개발에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세

### Django ModelForm vs. DRF Serializer

|          | Django    | DRF        |
| -------- | --------- | ---------- |
| Response | HTML      | JSON       |
| Model    | ModelForm | Serializer |



# 4. Single Model

## 4.1. DRF with Single Model

- 단일 모델의 data를 직렬화 (serialization) 하여 JSON으로 변환하는 방법에 대한 학습
- 단일 모델을 두고 CRUD 로직을 수행 가능하도록 설계
- API 개발을 위한 핵심 기능을 제공하는 도구 활용
  - **DRF built-in form**
  - [Postman](https://www.postman.com/)

### [참고] Postman

- API를 구축하고 사용하기 위해 여러 도구를 제공하는 API 플랫폼
- 설계, 테스트, 문서화 등의 도구를 제공함으로써 API를 더 빠르게 개발 및 생성 할 수 있도록 도움

## 4.2. Init Project

- 가상환경 설정 및 패키지 설치

```bash
$ python -m venv venv
$ source venv/Scripts/activate
$ pip install -r requirements.txt
```

- 설치된 앱 확인

```python
# settings.py

INSTALLED_APPS = [
    'articles',
    'django_seed',
    'django_extensions',
    'rest_framework',
]
```

- 작성된 url 확인

```python
# my_api/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('articles.urls')),
]
```

```python
# articles/urls.py

from django.urls import path
from . import views


urlpatterns = [

]
```

- 작성된 모델 확인

```python
# models.py

from django.db import models

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

- django-seed 라이브러리를 사용해 모델 구조에 맞는 데이터 생성

```bash
$ python manage.py migrate
$ python manage.py seed articles --number=20
```



## 4.3. ModelSerializer

- 모델 필드에 해당하는 필드가 있는 Serializer 클래스를 자동으로 만들 수 있는 shortcut
- 아래 핵심 기능을 제공
  1. 모델 정보에 맞춰 자동으로 필드 생성
  2. serializer에 대한 유효성 검사기를 자동으로 생성
  3. `.create()` & `.update()`의 간단한 기본 구현이 포함됨
- Model의 필드를 어떻게 '직렬화' 할 지 설정하는 것이 핵심
- 이 과정은 Django에서 Model의 필드를 설정하는 것과 동일함

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article

class ArticleListSerializer(serializers.ModelSerializer):

    class Meta:
        mode = Article
        fields = '__all__' #('id', 'title',) 도 가능
```

## 4.4. Serializer in Shell

- shell_plus에서 serializer 사용해보기

```
#1. shell_plus 실행
$ python manage.py shell_plus

#2. 작성한 Serializer import
>>> from articles.serializers

#3. 기본 인스턴스 구조 확인
>>> serializer = ArticleListSerializer()
>>> serializer
ArticleListSerializer():
	id = IntegerField(label='ID', read_only=True)
	title = CharField(max_length=100)
	

```

