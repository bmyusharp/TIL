# Model Relationship I

## Foreign Key

### 개념

- 외래 키 (외부 키)
- 관계형 데이터베이스에서 한 테이블의 필드 중 다른 테이블의 **행**을 식별할 수 있는 키
- 참조하는 테이블에서 속성(필드)에 해당하고, 이는 참조되는 테이블의 **기본 키(Primary Key)**를 가리킴
- 참조하는 테이블의 외래 키는 참조되는 테이블 **행 1개**에 대응됨
  - 이 때문에 참조하는 테이블에서 참조되는 테이블의 존재하지 않는 행을 참조할 수 없음
- 참조하는 테이블의 행 여러 개가 참조되는 테이블의 동일한 행을 참조할 수 있음

### 예시

- 게시글(Article)과 댓글(Comment)간의 모델 관계 설정

![image-20220413090910695](https://raw.githubusercontent.com/bmyusharp/TIL/master/img/image-20220413090910695.png)

게시글에 댓글 외래키를 다는 것이 아니라, 댓글에 게시글 외래키를 다는 것은 1대1 대응이 된다! (1:N, N:1)

위 사진의 경우, 1번 게시글에 2, 3번 댓글이 달려있다는 것을 의미한다.

### 특징

- 키를 사용하여 부모 테이블의 유일한 값을 참조 (참조 무결성)
- 외래 키의 값이 반드시 부모 테이블의 기본 키일 필요는 없지만 **유일한 값(=PK)**이어야 함
- [참고] 참조 무결성
  - 데이터베이스 관계 모델에서 관련된 2개의 테이블 간의 일관성을 말함
  - 외래 키가 선언된 테이블의 외래 키 속성(열)의 값은 그 테이블의 부모가 되는 테이블의 기본 키 값으로 존재해야 함 (게시글 테이블의 PK)

### field

- A many-to-one relationship
- 2개의 위치 인자가 반드시 필요
  1. 참조하는 model class
  2. on_delete 옵션
- migrate 작업 시 필드 이름에 _id를 추가하여 데이터베이스 열 이름을 만듦

- comment 모델 정의하기

```python
# articles/models.py

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    # 
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.content
```

### 'on_delete'

- 외래 키가 참조하는 객체가 사라졌을 때 외래 키를 가진 객체를 어떻게 처리할 지를 정의
- **Database Integrity(데이터 무결성)을 위해서 매우 중요한 설정**
- on_delete 옵션에 사용 가능한 값들
  - CASCADE: 부모 객체(참조된 객체)가 삭제 됐을 때 이를 참조하는 객체도 삭제
  - PROTECT, SET_NULL, SET_DEFAULT, SET(), DO_NOTHING, RESTRICT 등

### [참고] 데이터 무결성

- 데이터의 정확성과 일관성을 유지하고 보증하는 것을 가리키며, 데이터베이스나 RDBMS 시스템의 중요한 기능임
- 무결성 제한의 유형
  1. 개체 무결성 (Entity integrity)
     - PK의 개념과 관련
     - 모든 테이블이 PK를 가져야 하며 PK로 선택된 열은 고유한 값이어야 하고 빈 값은 허용치 않음을 규정
  2. **참조 무결성 (Referential integrity)**
     - FK (외래 키) 개념과 관련
     - FK 값이 데이터베이스의 특정 테이블의 PK 값을 참조하는 것
  3. 범위 무결성 (Domain integrity)
     - 정의된 형식(범위)에서 관계형 데이터베이스의 모든 컬럼이 선언되도록 규정

가급적 참조 테이블명의 소문자 단수형으로 작성할 것

### Migration

1. Migrations 생성

```bash
$ python manage.py makemigrations
```

2. migration 파일 확인
3. migrate

```bash
$ python manage.py migrate
```

4. articles_comment 테이블의 외래 키 컬럼 확인 (필드 이름에 **_id**가 추가됨)

```
>> article_id : bigint <<
```



### 데이터베이스의 ForeignKey 표현

- 만약 ForeignKey 인스턴스를 abcd로 생성 했다면 abcd_id로 만들어짐

- 하지만 명시적인 모델 관계 파악을 위해 참조하는 클래스 이름의 소문자(단수형) 으로 작성하는 것이 바람직함

### 댓글 생성 연습하기

```bash
$ python manage.py shell_plus 
```

shell_plus 실행(django 안에 install 되어있음)

```shell
In [1]: comment = Comment()

In [2]: commnet
Out[2]: <Commnet: >

In [3]: comment.content = 'first comment'

In [4]: comment.save()
------> Integrity Error (Not null)
# articles_comment 테이블의 
In [5]: article = Article.objects.create(title='title', content='content')

In [6]: article.pk
Out[6]: 1

In [7]: comment.content
Out[7]: 'first comment'

In [8]: comment.article = article

In [9]: comment.save() #실제로 세이브가 됨

In [10]: comment.pk
Out[10]: 1
```

comment`.article_id`= article.pk 와 같이 넣어줘야 함

comment`.article`= article 만 넣어줘도 됨

### 댓글 속성 값 확인

```shell
In [11]: comment.comment

In [12]: comment.article
Out[12]: <Article: title>

In [13]: comment.article.pk
Out[13]: 1

In [14]: comment.article.content
Out[14]:'content'
```

```shell
In [16]: comment = Comment(content='second comment', article=article)

In [17]: comment.save()

In [18]: comment.pk
Our[18]: 2

In [19]: comment.article.pk
Out[19]: 1

In [20]: comment.article_id
Out[20]: 1

comment.article_pk
------> AttributeError
```

shell 나가는 법: 'exit' 입력

### admin site에서 작성된 댓글 확인

```python
# articles/admin.py
from .models import Article, Comment
admin.site.register(Comment)
```

```bash
$ python manage.py createsuperuser
```

### 1:N 관계 related manager

- 역참조(**'comment_set'**)
  - Article(1) -> Comment(N)
  - article.comment 형태로는 사용할 수 없고, article**.comment_set** manager가 생성됨
    - (모델명)_set
    - 이 글에 어떤 댓글들이 달렸는지 보여주기 좋을 것이다.
  - 게시글에 몇 개의 댓글이 작성 되었는지 Django ORM이 보장할 수 없기 때문 (역참조를 못함)
    - article은 comment가 있을 수도 있고, 없을 수도 있음
    - **실제로 Article 클래스에는 Comment와의 어떠한 관계도 작성되어 있지 않음**
- 참조('article')
  - Comment(N) -> Article(1)
  - 댓글의 경우 어떠한 댓글이든 반드시 자신이 참조하고 있는 게시글이 있으므로, comment.article과 같이 접근할 수 있음
  - 실제 ForeignKeyField 또한 Comment 클래스에서 작성됨

### 연습

- dir() 함수를 통해 article 인스턴스가 사용할 수 있는 모든 속성, 메서드를 직접 확인하기

```shell
In [1]: article = Article.objects.get(pk=1)

In [2]: dir(article)
Out[2]:
['DoesNotExist',
'MultipleObjectsReturned',
'__class__',
'__delattr__',
...
'comment_set', <--- 역참조 모델명
...
]
```

- article의 입장에서 모든 댓글 조회하기 (역참조, 1->N)

```shell
In [3]: article.comment_set.all()
Out[3]: <QuerySet [<Comment: first comment>, <Comment: second comment>]>
```

- 조회한 모든 댓글 출력하기

```shell
In [4]: comments = article.comment_set.all()
In [5]: for comment in comments:
   ...: 	print(comment.content)
   ...:
first comment
second comment
```

- comment의 입장에서 참조하는 게시글 조회하기 (참조, N->1)

```shell
In [6]: comment = Comment.objects.get(pk=1)

In [7]: comment.article
Out[7]: <Article: title>

In [8]: comment.article.content
Out[8]: 'content'

In [9]: comment.article_id
Out[9]: 1
```

### ForeignKey arguments - 'related_name'

- 역참조 시 사용할 이름('model_set' manager)을 변경할 수 있는 옵션

```python
# articles/models.py

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comments')
...
```

- 위와 같이 변경 하면 article.comment_set은 더이상 사용할 수 없고, **article.comments**로 대체됨
- [주의] 역참조 시 사용할 이름 수정 후, migration 과정 필요

그러나, 그냥 두는 게 좋습니다. (향후 M:N 데이터관계를 위해)



## Comment CREATE

### CommentForm 작성

```python
# articles/forms.py

from .models import Article, Comment

class CommentForm(forms.ModleForm):
    
    class Meta:
        model = Comment
        fields = '__all__'
```

```python
# articles/views.py

from .forms import ArticleForm, CommentForm

@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm() # 이 부분 추가
    context = {
        'article': article,
        'comment_form': comment_form, # 이 부분 추가
    }
    ...
```

```html
<!-- articles/detail.html -->
...
<form action="" method="POST">
  {% csrf_token %}
  {{ comment_form }}
  <input type="submit">
</form>
```

- ForeignKeyField를 작성자가 직접 입력해야 하는 상황이 발생 (=어느 글에 댓글을 작성할 건지를 직접 선택해야 되는 상황)
- CommentForm에서 외래 키 필드 출력 제외

```python
# articles/forms.py

class CommentForm(forms.ModelForm):
    
    class Meta:
        model = Comment
        fields = ('article',)
```

### 댓글 작성 로직

```python
# articles/urls.py
...
	path('<int:pk>/comments/', views.comments_create, name='comments_create'),
...
```

```html
<!-- articles/detail.html -->
<form action="{% url 'articles:comments_create' article.pk %}" method="POST">
  {% csrf_token %}
  {{ comment_form }}
  <input type="submit">
</form>
```

```python
# articles/views.py

@require_POST
def comments_create(request, pk):
    # article = Article.objects.get(pk=pk)
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid():
        comment = comment_form.save(commit=False) 
        #commit=False로 한 이유(기본값은 True였고): 세이브 안 하고 객체만 반환 즉, 아직 저장을 안 하고, 인스턴스만 제공하겠다는 것
        comment.article = article
        comment.save()
    return redirect('articles:detail', article.pk)
# else는 필요 없는 이유: 페이지는 detail에서 오고 있기 때문에
```

### The 'save()' method

- save(commit=False)
  - Create, but don't save the new instance
  - 아직 **데이터베이스에 저장되지 않은 인스턴스를 반환**
  - 저장하기 전에 **객체에 대한 사용자 지정 처리를 수행할 때** 유용하게 사용



## Comment READ

### 댓글 출력

- 특정 article에 있는 모든 댓글을 가져온 후 context에 추가

```python
# articles/views.py
from .models import Article, Comment

def detail(request, pk):
    ...
    # 조회한 article의 모든 댓글을 조회 (역참조)
    comments = article.comment_set.all()
    context = {
        'article': article,
        'comment_form': comment_form,
        'comments': commtents,
    }
    ...
```

```html
<!-- articles/detail.html -->
...
<hr>
<h4>댓글 목록</h4>
<ul>
  {% for comment in comments %}
    <li>{{ comment.content }}</li>
  {% endfor %}
</ul>
<hr>
```



## Comment DELETE

```python
# articles/urls.py

...
	path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
...
```

```html
<!-- articles/detail.html -->
...
<h4>댓글 목록</h4>
<ul>
  {% for comment in comments %}
    <li>
      {{ comment.content }}
      <form
        action="{% url 'articles:comments_delete' article.pk comment.pk %}"
        method="POST"
        class="d-inline"
      >
        {% csrf_token %}
        <input type="submit" value="DELETE">
        </form>
      </form>
    </li>
  {% endfor %}
</ul>
<hr>
...
```

```python
# articles/views.py

def comment_delete(request, article_pk, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    comment.delete()
    return redirect('articles:detail', article_pk)
```

### 인증된 사용자의 경우만 댓글 작성 및 삭제

```python
# articles/views.py

@require_POST
def comments_create(request, pk):
    if request.user.is_authenticated: # 추가된 부분
        ...
        return redirect('articles:detail', article.pk)
    return redirect('acounts:login') # 추가된 부분

@require_POST
def comments_delete(request, article_pk, comment,pk):
    if request.user.is_authenticated: # 추가된 부분
        comment = get_object_or_404(Comment, pk=comment=pk)
        ...
```



## Comment 추가사항

### 댓글 개수 출력하기

```python
# 1. {{ comments|length }}
# 2. {{ article.comment_set.all|length }}
# 3. {{ comments.count }}
```

```html
<!-- articles/detail.html -->

<h4>댓글 목록</h4>
{% if comments %}
  <p><b>댓글 {{ comments|length }}</b></p>
{% endif %}
```



### 댓글이 없는 경우 대체 컨텐츠 출력

```html
<!-- articles/detail.html -->
<ul>
  {% for comment in comments %}
    <li>
      {{ comment.content }}
      <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method="POST" class="d-line">
        {% csrf_token %}
        <input type="submit" value="DELETE">
      </form>
    </li>
  {% empty %}
    <p>댓글이 없어요..</p>
  {% endfor %}
</ul>
```





# Customizing authentication in Django

## Substituting a custom User model

### User 모델 대체하기

- 일부 프로젝트에서는 Django의 **내장 User모델이 제공하는 인증 요구사항이 적절하지 않을 수 있음**
  - ex) username 대신 email을 식별 토큰으로 사용하는 것이 더 적합한 사이트
- Django는 User를 참조하는데 사용하는 **AUTH_USER_MODEL** 값을 제공하여, default user model을 **재정의(override)** 할 수 있도록 함
- Django는 새 프로젝트를 시작하는 경우 기본 사용자 모델이 충분하더라도, **커스텀 유저 모델을 설정하는 것을 강력하게 권장** (highly recommended)
  - **단, 프로젝트의 모든 migrations 혹은 첫 migrate를 실행하기 전에 이 작업을 마쳐야 함**

### AUTH_USER_MODEL

- User를 나타내는데 사용하는 모델
- 프로젝트가 **진행되는 동안 변경할 수 없음**
- 프로젝트 시작 시 설정하기 위한 것이며, 참조하는 모델은 첫번째 마이그레이션에서 사용할 수 있어야 함
- 기본 값: 'auth.User' (auth 앱의 User 모델)

- [참고] 프로젝트 중간(mid-project)에 AUTH_USER_MODEL 변경하기
  - 모델 관계에 영향을 미치기 때문에 훨씬 더 어려운 작업이 필요
  - 즉, 중간 변경은 권장하지 않으므로 초기에 설정하는 것을 권장

### Custom User 모델 정의하기

- 관리자 권한과 함께 완전한 기능을 갖춘 User 모델을 구현하는 기본 클래스인 AbstractUser를 상속받아 새로운 User 모델 작성

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

`앞으로는 무조건 하세요`

- 기존에 Django가 사용하는 User 모델이었던 auth 앱의 User 모델을 accounts 앱의 User 모델을 사용하도록 변경

```python
# settings.py

AUTH_USER_MODEL = 'accounts.User'
```

- admin site에 Custom User 모델 등록

```python
# accounts/admin.py

from django.contrib.auth.admin import UserAdmin
from .models import User

admin.site.register(User, UserAdmin)
```



- 프로젝트 중간에 진행했기 때문에 데이터베이스를 초기화 한 후 마이그레이션 진행
- 초기화 방법
  1. 먼저, db.sqlite3 파일 삭제
  2. migrations 파일 모두 삭제 (단, **파일명에 숫자가 붙은 파일만** 삭제)

```bash
$ python manage.py makemigrations

$ python manage.py migrate
```



## Custom user & Built-in auth forms

### 회원가입 시도 후 아래와 같은 에러 발생

> AttributeError at /accounts/signup/
>
> Manager isn't available; 'auth.User' has been swapped for 'accounts.User'

- UserCreationForm과 UserChangeForm은 기존 내장 User 모델을 사용한 ModelForm이기 때문에 커스텀 User 모델로 대체해야 함

### Custom Built-in Auth Forms

- 기존 User 모델을 사용하기 때문에 커스텀 User 모델로 다시 작성하거나 확장해야 하는 forms
  - UserCreationForm
  - UserChangeForm

- 이처럼 커스텀 User 모델이 AbstractUser의 하위 클래스인 경우 다음과 같은 방식으로 form을 확장

```python
from django.contrib.auth.forms import UserCreationForm
from myapp.models import CustomUser

class CustomUserCreationForm(UserCreationForm):
    
    class Meta(UserCreationForm.Meta):
        model = Customer
        fields = UserCreationForm.Meta.fields + ('custom_field',)
```

### User CreationForm 확장

```python
# accounts/forms.py

from django.contrib.auth.forms import UserChangeForm, UserCreationForm

class CustomUserCreationForm(UserCreationForm):
    
    class Meta(UserCreationForm.Meta):
        model = get_user_model()
        fields = UserCreationForm.Meta.fields + ('email',)
```

### signup view 함수 코드 수정

- 수정 후 회원가입 재시도

```python
# accounts/views.py


```

### get_user_model()

- **현재 프로젝트**에서 활성화된 사용자 모델(activate user model)을 반환
  - user 모델을 **커스터마이징한 상황**에서는 Custom User 모델을 반환

- 이 때문에 Django는 User 클래스를 직접 참조하는 대신

  django.contrib.auth.get_user_model()을 사용하여 참조해야 한다고 강조





# Model Relationship II

## 1:N 관계 설정

### User - Article (1:N)

#### User 모델 참조하기

1. **settings.AUTH_USER_MODEL** -> str
   - User 모델에 대한 외래 키 또는 다대다 관계를 정의 할 때 사용해야 함
   - models.py에서 User 모델을 참조할 때 사용
2. **get_user_model()** -> object
   - 현재 활성화(activate) 된 User 모델을 반환
     - 커스터마이징한 User 모델이 있을 경우는 Custom User 모델, 그렇지 않으면 User를 반환
     - User를 직접 참조하지 않는 이유

#### User와 Article 간 모델 관계 정리 후

```python
# User와 Article 간 모델 관계 정의 후 migration
# articles/models.py

from django.conf import settings

class Article(models.Model)
	user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

- null 값이 허용되지 않는 user-id 필드가 별도의 값 없이 article에 추가되려 하기 때문
- 1을 입력 후 enter
  - 현재 화면에서 기본 값을 설정하겠다 라는 의미
- 1을 입력 후 enter
  - 기존 테이블에 추가되는 user_id 필드의 값을 1로 설정하겠다 라는 의미

![image-20220413150423200](https://raw.githubusercontent.com/bmyusharp/TIL/master/img/image-20220413150423200.png)

- migrate 과정 마무리

```bash
$ python manage.py migrate
```

#### 게시글 출력 필드 수정

- 게시글 작성 페이지에서 불필요한 필드가 출력되는 것을 확인
- ArticleForm의 출력 필드 수정 후 게시글 작성 재시도

```python
# articles/forms.py
class ArticleForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = ('title', 'content',)
```

- 게시글 작성 시 NOT NULL constraint failed: articles_article.user_id 에러 발생
- 게시글 작성 시 작성자 정보가 누락되었기 때문

#### CREATE

- 게시글 작성 시 작성자 정보(article.user) 추가 후 게시글 작성 재시도

```python
# articles/views.py

@login_required
@require_http_methods(['GET', 'POST'])
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save(commit=False) #
            article.user = request.user #
            article.save()
            return redirect('articles:detail', article.pk)
```

#### DELETE

- 자신이 작성한 게시글만 삭제 가능하도록 설정

```python
# articles/views.py

@require_POST
def delete(request, pk):
    article = get_object_or_404(Articls, pk=pk)
    if request.user.is_authenticated:
        if request.user == article.user: #
            article.delete()
            return redirect('articles:index')
    return redirect('articles:detail', article.pk) #
```

#### UPDATE

- 자신이 작성한 게시글만 수정 가능하도록 설정

```python
# articles/views.py

@login_required
@require_http_methods(['GET', 'POST'])
def update(request, pk):
    article = get_object_or_404(Article, pk=pk)
    if request.user == article.user: #
        if request.method == 'POST':
            form = ArticleForm(request.POST, instance=article)
            if form.is_valid():
                form.save()
                return redirect('articles:detail', article.pk)
        else:
            form = ArticleForm(instance=article)
    else:
        return redirect('articles:index') #
    context = {
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```

#### READ

- 게시글 작성 user가 누구인지 index.html에서 출력하기

```html
<!-- articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  ...
  {% for article in articles %}
	<p><b>작성자: {{ article.uesr }}</b></p>
	<p>글 번호: {{ article.pk }}</p>
	<p>글 제목: {{ article.title }}</p>
	<p>글 내용: {{ article.content }}</p>
	<a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
	<hr>
  {% endfor %}
{% endblock %}
```

- 해당 게시글의 작성자가 아니라면, 수정/삭제 버튼을 출력하지 않도록 처리

```html
<!-- articles/detail.html -->

{% if user == article.user %}
  <a href="{% url 'articles:update' article.pk %}">[UPDATE]</a>
  <form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="DELETE">
  </form>
{% endif %}
```



### User - Comment

#### User와 Comment 간 모델 관계 정의 후 migration

```python
# articles/models.py

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE) #
    ...
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

- null 값이 허용되지 않는 user_id 필드가 별도의 값 없이 comment 에 추가되려 하기 때문

- 1을 입력 후 enter

  - '현재 화면에서 기본 값을 설정하겠다'라는 의미

- 1을 입력 후 enter

  - '기존 테이블에 추가되는 user_id 필드의 값을 1로 설정하겠다'라는 의미

    (기존 댓글의 작성자가 모두 1번 user가 됨)

- migrate 과정 마무리

```bash
$ python manage.py migrate
```

#### 댓글 출력 필드 수정

- 게시글 작성 페이지에서 불필요한 필드가 출력되는 것을 확인

- 댓글 작성 시 user ForeignKeyField를 출력하지 않도록 설정

```python
# articles/forms.py

class CommentForm(forms.ModelForm):
    
    class Meta:
        model = Comment
        exclude = ('article', 'user',) # user 부분
```

- 댓글 작성 시 NOT NULL constraint failed: articles_comment.user_id 에러 발생
- 댓글 작성 시 작성자 정보(comment.user)가 누락되었기 때문

#### CREATE

- 댓글 작성 시 작성자 정보(request.user) 추가 후 댓글 작성 재시도

```python
# articles/views.py

@require_POST
def comments_create(request, pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            comment = comment_form.save(commit=False)
            comment.article = article
            comment.user = request.user #
            comment.save()
    ...
```

#### READ

- 비로그인 유저에게는 댓글 form 출력 숨기기

```html
<!-- articles/detail.html -->

{% if request.user.is_authenticated %}
  <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
    {% csrf_token %}
    {{ comment_form }}
    <input type="submit">
  </form>
{% else %}
  <a herf="{% url 'accounts:login' %}">[댓글을 작성하려면 로그인하세요.]</a>
{% endif %}
```

- 댓글 작성자 출력하기

```html
<!-- articles/detail.html -->

{% for comment in comments %}
  <li>
    {{ comment.user }} - {{ comment.content }}
    <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method="POST" class="d-inline">
      {% csrf_token %}
      <input type="submit" value="DELETE">
    </form>
  </li>
{% empty %}
  <p>댓글이 없어요..</p>
{% endfor %}
```

#### DELETE

- 자신이 작성한 댓글만 삭제 버튼을 볼 수 있도록 수정

```html
<!-- articles/detail.html -->

{% for comment in comments %}
  <li>
    {{ comment.user }} - {{ comment.content }}
    {% if user == comment.user %}
      <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method="POST" class="d-inline">
        {% csrf_token %}
        <input type="submit" value="DELETE">
      </form>
    {% endif %}
  </li>
{% empty %}
  <p>댓글이 없어요..</p>
{% endfor %}
```

```python
# articles/views.py

@require_POST
def comments_delete(request, article_pk, comment_pk):
    if request.user.is_authenticated:
        comment = get_object_or_404(Comment, pk=pk)
        if request.user == comment.user: #
            comment.delete()
    return redirect('articles:detail', article.pk)
```

