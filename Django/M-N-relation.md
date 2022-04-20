# Model relationship

오늘 배우는 게 끝나면...

- 좋아요 기능, 프로필 페이지, 팔로우 기능 구현가능!

## Intro: 병원 진료 기록 시스템

- 환자와 의사가 사용하는 병원 진료 기록 시스템 구축
  - 병원 시스템에서 가장 핵심이 되는 객체는 무엇일까? - 환자와 의사
  - 이 둘의 관계를 어떻게 표현할 수 있을까?

- 시작하기 전
  - 모델링은 현실 세계를 최대한 유사하게 반영하기 위한 것
  - 우리 일상에 가까운 예시를 통해 DB를 모델링하고, 그 내부에서 일어나는 데이터의 흐름을 어떻게 제어할 수 있을지 고민해보기



### 1. 1:N의 한계

- 1:N 모델 관계 설정

```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- 마이그레이션 후 shell_plus 실행

```bash
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py shell_plus
```

- 의사 2명과 환자 2명 생성

```shell
In [1]: doctor1 = Doctor.objects.create(name='justin')

In [2]: doctor2 = Doctor.objects.create(name='eric')

In [3]: patient1 = Patient.objects.create(name='tony', doctor=doctor1)

In [4]: patient2 = Patient.objects.create(name='harry', doctor=doctor2)

In [5]: doctor1
Out[5]: <Doctor: 1번 의사 justin>

In [6]: doctor2
Out[6]: <Doctor: 2번 의사 eric>

In [7]: patient1
Out[7]: <Patient: 1번 환자 tony>

In [8]: patient2
Out[8]: <Patient: 2번 환자 harry>
```

![image-20220418102314652](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418102314652.png)

- 1번 환자(tony)가 1번 의사의 진료를 마치고, 2번 의사에게도 방문하려고 한다면, 새로운 예약을 생성해야 한다.

- 기존의 예약을 유지한 상태로 새로운 예약을 생성
- 새로 생성한 3번 환자(tony)는 1번 환자(tony)와 다름

```shell
In [9]: patient3 = Patient.objects.create(name='tony', doctor=doctor2)

In [10]: patient3
Out[10]: <Patient: 3번 환자 tony>
```

- 한 번에 두 의사에게 진료를 받고자 함
- 하나의 외래 키에 2개의 의사 데이터를 넣을 수 없음

![image-20220418102604601](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418102604601.png)

```shell
In [11]: patient4 = Patient.objects.create(name='harry', doctor=doctor1, doctor2)
  File "<ipython-input-11-6edaf3ffb4e6>", line 1
    patient4 = Patient.objects.create(name='harry', doctor=doctor1, doctor2)
                                                                           ^
SyntaxError: positional argument follows keyword argument
```

- 새로운 예약을 생성하는 것이 불가능
  - 새로운 객체를 생성해야 함
- 여러 의사에게 진료 받은 기록을 환자 한 명에 저장할 수 없음
  - 외래 키에 '1, 2' 형식의 데이터를 사용 할 수 없음



### 2. 중개 모델

- 중개 모델 (혹은 중개 테이블, Associative Table) 작성

```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'

# 외래키 삭제
class Patient(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

# 중개모델 작성
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'
```

- 데이터베이스 초기화 / 마이그레이션 및 shell_plus 실행

![image-20220418103052753](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418103052753.png)

- 중개 모델과의 모델 관계 확인

![image-20220418103142514](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418103142514.png)

- 의사 1명과 환자 1명 생성 및 예약 생성

```shell
In [1]: doctor1 = Doctor.objects.create(name='justin')

In [2]: patient1 = Patient.objects.create(name='tony')

In [3]: Reservation.objects.create(doctor=doctor1, patient=patient1) # 예약방식이 달라짐
Out[3]: <Reservation: 1번 의사의 1번 환자>
# 각자 1에서 N으로 역참조하는 것 (의사 입장에서 보건, 환자 입장에서 보건)
# 그것을 중개 모델을 통해 하는 것입니다.

In [4]: doctor1.reservation_set.all()
Out[4]: <QuerySet [<Reservation: 1번 의사의 1번 환자>]>

In [5]: patient1.reservation_set.all()
Out[5]: <QuerySet [<Reservation: 1번 의사의 1번 환자>]>

In [6]: patient2 = Patient.objects.create(name='harry')

In [7]: Reservation.objects.create(doctor=doctor1, patient=patient2)
Out[7]: <Reservation: 1번 의사의 2번 환자>

In [8]: doctor1.reservation_set.all()
Out[8]: <QuerySet [<Reservation: 1번 의사의 1번 환자>, <Reservation: 1번 의사의 2번 환자>]> # 의사 입장에서 환자 조회 (역참조)

In [9]: patient2.reservation_set.all()
Out[9]: <QuerySet [<Reservation: 1번 의사의 2번 환자>]> # 환자 입장에서 의사 조회 (역참조)
```

- 의사의 예약 환자 조회

```shell
In [10]: for reservation in doctor1.reservation_set.all():
    ...: 	print(reservation.patitent.name)
tony
harry
```



### 3. ManyToManyField

- 다대다 (M:N, many-to-many) 관계 설정 시 사용하는 모델 필드
- 하나의 필수 위치인자 (M:N 관계로 설정할 모델 클래스) 가 필요

- ManyToManyField 작성 (중개 모델 삭제)
  - 필드 작성 위치는 Doctor 또는 Patient 모두 작성 가능

```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    # ManyToManyField 작성
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- 데이터베이스 초기화 / 마이그레이션 및 shell_plus 실행
- ManyToManyField로 인해 생성된 중개 테이블 확인

![image-20220418104237665](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418104237665.png)

reservation 테이블만 없어짐. 그리고, patient 테이블에 외래키가 생긴 것도 아님. 중개 테이블이 patient의 ManyToManyField로 인해 생겼다는 것이다. (따로 만들 필요 없이)

이 경우, Doctor->Patient는 역참조, Patient->Doctor는 참조이다. `doctors = models.ManyToManyField(Doctor)`에 의해

- 이것을 doctor 테이블에 만들면? 중개테이블의 이름만 바뀔 뿐, 상관은 없다! 
  - 1:N 관계에서 외래키가 무조건 N쪽에 있어야 했던 것과는 다른 부분이다.

- 의사 1명과 환자 2명 생성

```shell
In [1]: doctor1 = Doctor.objects.create(name='justin')

In [2]: patient1 = Patient.objects.create(name='tony')

In [3]: patient2 = Patient.objects.create(name='harry')
```

- 예약 생성
  1. patient1(tony)이 doctor1(justin)에게 예약
  2. patient1이 예약한 의사 목록 확인
  3. doctor1에게 예약된 환자 목록 확인

```shell
In [4]: patient1.doctors.add(doctor1) 
# .add(의사 객체) - 환자가 스스로 예약을 만드는 것 처럼 보임

In [5]: patient1.doctors.add(doctor1)

In [6]: patient1.doctors.all()
Out[6]: <QuerySet [<Doctor: 1번 의사 justin>]>

In [7]: doctor1.patient_set.all()
Out[7]: <QuerySet [<Patient: 1번 환자 tony>]>
# 역참조가 되어도 만들어지는 테이블도 똑같고 결과도 똑같음

```

![image-20220418105249174](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418105249174.png)

... ( 그 동안 환자 2가 추가됨 )

- 예약 삭제 (역참조)
  1. doctor1(justin)이 patient(tony) 진료 예약 취소
  2. doctor1에게 예약된 환자 목록 확인
  3. patient1이 예약한 의사 목록 확인

```shell
In [10]: doctor1.patient_set.remove(patient1)

In [11]: doctor1.patient_set.all()
Out[11]: <QuerySet [<Patient: 2번 환자 harry>]>

In [12]: patient1.doctors.all()
Out[12]: <QuerySet []>
```



### 4. related_name

- target model (관계 필드를 가지지 않은 모델)이 source model (관계 필드를 가진 모델) 을 참조할 때 사용할 manager의 이름을 설정
- 즉, 역참조 시에 사용하는 manager의 이름을 설정
- ForeignKey의 related_name과 동일 (단, 외래키에서는 잘 사용하지 않음)

```python
# hospitals/models.py

class Patient(models.Model):
    # ManyToManyField - related_name 작성
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    name = models.TextField()
```

`patient_set`을 `patients`로 이름 설정

- 마이그레이션 및 shell_plus 실행 (related_name 때문에 초기화할 필요는 없음)

- doctor1의 예약 환자 목록 확인 해보기 (역참조)
- related_name 설정 후 기존의 _set manager는 더 이상 사용할 수 없음

 ```shell
 In [1]: doctor1 = Doctor.objects.get(pk=1)
 
 In [2]: doctor1.patients.all()
 Out[2]: <QuerySet []>
 
 In [3]: doctor1.patient_set.all()
 ---------------------------------------------
 AttributeError
 ...
 'Doctor' object has no attribute 'patient_set'
 # 이제는 아래로 써도 된다.(역참조 참조 고민하지 않아도 된다)
 doctor1.patients.all()
 patient1.doctors.all()
 ```



### 중개 모델(테이블) in Django

- Django는 ManyToManyField를 통해 중개 테이블을 자동으로 생성

  ManyToManyField의 한계?: 의사-환자 외에도 몇시 예약인지, 무슨 예약인지를 표시할 수 있을까?

  __-> 직접 작성해야 한다...__

- 그렇다면 중개 테이블을 직접 작성하는 경우는 없을까?

  - 중개 테이블을 수동으로 지정하려는 경우 through 옵션을 사용하여, 중개 테이블을 나타내는 Django 모델을 지정할 수 있음 (다음 챕터에서 확인)
  - 가장 일반적인 용도는 중개 테이블에 추가 데이터를 사용해 다대다 관계로 연결하려는 경우에 사용

### 요약

- 실제 Doctor와 Patient 테이블이 변하는 것은 없음
- 1:N 관계는 완전한 종속의 관계이지만, **M:N 관계는 의사에게 진찰받는 환자, 환자를 진찰하는 의사의 두 가지 형태로 모두 표현이 가능한 것**



## ManyToManyField

### 개념 및 특징

- 다대다(M:N) 관계 설정 시 사용하는 모델 필드
- 하나의 필수 위치인자(M:N 관계로 설정할 모델 클래스)가 필요
- 모델 필드의 RelatedManager를 사용하여 관련 개체를 추가, 제거 또는 만들 수 있음
  - **add(), remove(),** create(), clear() ...

- [참고] RelatedManager
  - 일대다 또는 다대다 관련 컨텍스트에서 사용되는 manager

### Arguments

1. related_name
2. through
   - 중개 테이블을 직접 작성하는 경우, through 옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정할 수 있음
   - 일반적으로 중개 테이블에 추가 데이터를 사용하는 다대다 관계와 연결하려는 경우 (추가적으로 중개모델에 데이터가 있을 경우 직접 작성하는 것)
3. symmertical



### Related Manager

- 1:N 또는 M:N 관련 컨텍스트에서 사용되는 매니저
- 같은 이름의 메서드여도 각 관계에 따라 다르게 사용 및 동작
- 메서드 종류
  - add(), remove(), ...

- **add()**
  - "지정된 객체를 관련 객체 집합에 추가"
  - 이미 존재하는 관계에 사용하면 관계가 복제되지 않음
  - 모델 인스턴스, 필드 값(PK)을 인자로 허용

![image-20220418111634870](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418111634870.png)

- **remove()**
  - "관련 객체 집합에서 지정된 모델 객체를 제거"
  - 내부적으로 QuerySet.delete()를 사용하여 관계가 삭제됨
  - 모델 인스턴스, 필드 값(PK)을 인자로 허용

![image-20220418114715365](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418114715365.png)



### through 예시

- 모델 관계 설정

```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation')
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)
    # 증상과 예약시간 추가
    # 일반적인 ManyToManyField만으로는 안되고, 추가로 중개 테이블을 직접 작성해야 함.

    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```

- 데이터베이스 초기화 / 마이그레이션 및 shell_plus 실행

- 중개 테이블 확인

![image-20220418112344128](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418112344128.png)

- 의사 1명과 환자 2명 생성

```shell
In [1]: doctor1 = Doctor.objects.create(name='justin')

In [2]: patient1 = Patient.objects.create(name='tony')

In [3]: patient2 = Patient.objects.create(name='harry')
```

- 예약 생성 - 1

```shell
In [4]: reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')

In [5]: reservation1.save()
# Reservation 클래스로 예약 테이블을 객체처럼 다룰 수 있게 됨

In [6]: doctor1.patient_set.all()
Out[6]: <QuerySet [<Patient: 1번 환자 tony>]>

In [7]: patient1.doctors.all()
Out[7]: <QuerySet [<Doctor: 1번 의사 justin>]>

In [8]: reservation1
Out[8]: <Reservation: 1번 의사의 1번 환자>
```

- 중개 테이블 확인

![image-20220418112702699](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418112702699.png)

Q. 예약을 의사 입장에서, 환자 입장에서 만들 수 있을까?

- 예약 생성 - 2
  - through_defaults를 사용해 add(), create() 또는 set()을 사용하여 관계를 생성

```shell
In [9]: patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
```

- 중개 테이블 확인

![image-20220418112959932](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418112959932.png)

```shell
In [10]: doctor1.patient_set.all()
Out[10]: <QuerySet [<Patient: 1번 환자 tony>, <Patient: 2번 환자 harry>]>

In [11]: patient1.doctors.all()
Out[11]: <QuerySet [<Doctor: 1번 의사 justin>]>
```



### 데이터베이스에서의 표현

- Django는 다대다 관계를 나타내는 중개 테이블을 만듦
- 테이블 이름은 다대다 필드의 이름과 이를 포함하는 모델의 테이블 이름을 조합하여 생성됨



### 중개 테이블의 필드 생성 규칙

1. source model 및 target model 모델이 다른 경우
   - id
   - <containing_model>_id `patient_id`
   - <other_model>_id (e.g. `doctor_id`)
2. ManyToManyField 가 동일한 모델을 가리키는 경우 (자기자신=재귀)
   - id
   - from\_<model>_id
   - to\_<model>_id



## Like
### Like 구현하기
- django_model_relationship_II 프로젝트로 진행
- ManyToManyField 작성 후 마이그레이션

```python
# articles/models.py

from django.db import models
from django.conf import settings

# Create your models here.
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title


class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

```bash
$ python manage.py makemigrations
```

> 잠시...
>
> '좋아요(Like)'는 누구와 누구의 모델관계?
>
> 유저가, 게시글에! (1:N일까?)
>
> 게시글에는 여러 유저가 좋아요를 누를 수 있고,
>
> 유저는, 여러 게시글에 좋아요를 누를 수 있다.
>
> 
>
> 1번 유저가 2,3,4번에 좋아요를 눌렀다 -> 어떻게 저장할 것인가?
>
> 즉, Article과 User의 M:N 관계이다.

빨간 ERRORS 발생

HINT: related_name 인자를 정하거나, 수정 - 두 줄의 user (1:N(작성자:작성글) 에서와 M:N(좋아요 한 유저:게시글)) 에서 문제 발생한 것

- like_users 필드 생성 시 자동으로 역참조는 .article_set 매니저를 생성
- 그러나 이전 1:N 관계에서 이미 해당 매니저 이름을 사용 중이기 때문
- User와 관계된 ForeignKey 또는 MTMF 중 하나에 related_name 추가 필요

```python
...
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
...
```

![image-20220418141512747](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418141512747.png)

> 현재 User - Article 간 사용 가능한 DB API

- a
- a
- u
- u





- url 작성

```python
# articles/urls.py

urlpatterns = [
    ...
    path('<int:article_pk>/likes/', views.likes, name='likes'),
]
```

- like view 함수 작성

```python
# articles/views.py
...
@require_POST
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)

        if request.user in article.like_users.all():
            article.like_users.remove(request.user)

        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('accounts:login')
```

- index 페이지에 like 출력 부분 작성

```html
<!-- articles/index.html -->
...
<p>글 내용: {{ article.content }}</p>
    <div>
      <form action="{% url 'articles:likes' article.pk %}" method="POST">
        {% csrf_token %}
        {% if user in article.like_users.all %}
          <input type="submit" value="좋아요 취소">
        {% else %}
          <input type="submit" value="좋아요">
        {% endif %}
      </form>
    </div>
    <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
...
```



### QuerySetAPI - 'exist()'

- QuerySet에 결과가 포함되어 있으면 True를 반환하고 그렇지 않으면 False를 반환
- 특히 규모가 큰 QuerySet의 컨텍스트에서 특정 개체 존재 여부와 관련된 검색에 유용
- 고유한 필드 (예: pk) 가 있는 모델이 QuerySet의 구성원인지 여부를 찾는 가장 효율적인 방법



## Profile Page

- 자연스러운 follow 흐름을 위한 회원 프로필 페이지 작성하기

- url 작성

```python
# accounts/urls.py
...
urlpatterns = [
    path('<username>/', views.profile, name='profile'),
] # 문자열을 그대로 쓴다고? 이 path 주소가 위로 올라가지 않게 조심할 것 (위에서부터 찾기 때문)(잘못하면 하루종일 뒤질 수도 있다)
```

- profile view 함수 작성

```python
# accounts.views.py
from django.contrib.auth import get_user_model
...
def profile(request, username):
    person = get_object_or_404(get_user_model(), username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```

- profile 페이지 작성

```html
<!-- accounts/profile.html -->
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}님의 프로필</h1>

<hr>

{% comment %}이 사람이 작성한 게시글 목록{% endcomment %}
<h2>{{ person.username }}이 작성한 게시글</h2>
{% for article in person.article_set.all %}
  <p>{{ article.title }}</p>
{% endfor %}

<hr>

{% comment %}이 사람이 작성한 댓글 목록{% endcomment %}
<h2>{{ person.username }}이 작성한 게시글</h2>
{% for comment in person.comment_set.all %}
  <p>{{ comment.content }}</p>
{% endfor %}

<hr>

{% comment %}이 사람이 좋아요를 누른 게시글 목록{% endcomment %}
<h2>{{ person.username }}이 좋아요를 누른 게시글</h2>
{% for article in person.like_articles.all %}
  <p>{{ article.title }}</p>
{% endfor %}
{% endblock content %}
```

```html
<!-- base.html -->
...
<h3>Hello, {{ user }}</h3>
      <a href="{% url 'accounts:profile' request.user.username %}">내 프로필</a>
      <form action="{% url 'accounts:logout' %}" method="POST">
...
```



## Follow

### Follow 구현

- MTMF 작성 후 마이그레이션

```python
# accounts/models.py

class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

참조와 역참조 둘 다 작성해줘야 함

symmetrical 속성을 사용함

- **symmetrical**
  - ManyToManyField가 동일한 모델(on self)을 가리키는 정의에서만 사용
  - symmetrical=True(기본값)일 경우 Django는 person_set 매니저를 추가하지 않음
  - source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면, target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함
    - 즉, 내가 당신의 친구라면 당신도 내 친구가 되는 것 (페이스북 친구)
    - 대칭을 원하지 않는 경우 False 로 설정
    - Follow 기능 구현에서 다시 확인할 것



- 생성된 중개 테이블 확인

![image-20220418151354974](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220418151354974.png)

-> from_ 과 to_ 가 생김 

- url 작성

```python
# accounts/urls.py
...
	path('<int:user_pk>/follow/', views.follow, name='follow'),
```

- follow view 함수 작성

```python
# accounts/views.py
# request.user => me user
# get_object_or_404 => you 로 변환해서 작업하면 헷갈리지 않음.

@require_POST
def follow(request, user_pk):
    if request.user.is_authenticated:
        person = get_object_or_404(get_user_model(), pk=user_pk)
        if person != request.user:
            if person.followers.filter(pk=request.user.pk).exists():
                person.followers.remove(request.user)
            else:
                person.followers.add(request.user)
        return redirect('accounts:profile', person.username)
    return redirect('accounts:login')
```

- profile 페이지에 팔로우와 언팔로우 버튼 작성
  1. 팔로잉 / 팔로워 수 출력
  2. 자기 자신을 팔로우 할 수 없음

```html
<!-- accounts/profile.html -->
<!-- 상단 부분 -->
<div>
  <div>
    팔로우 {{ person.followings.all|length }} | 팔로워 {{ person.followers.all|length }}
  </div>
  {% if user != person %}
    <div>
      <form action="{% url 'accounts:follow' person.pk %}" method="POST">
        {% csrf_token %}
        {% if user in person.followers.all %}
          <input type="submit" value="언팔로우">
        {% else %}
          <input type="submit" value="팔로우">
        {% endif %}
      </form>
    </div>
  {% endif %}
</div>
```

팁. 아래 코드는 위 코드와 완벽하게 똑같이 작동함

with 작명구문=원래구문 으로 with 블록 내에서 긴 이름을 줄여부를 수 있음

단, 최적화되는 것은 아님.

```html
{% with followers=person.followers.all followings=person.followings.all %}
<div>
  <div>
    팔로우 {{ followings|length }} | 팔로워 {{ followers|length }}
  </div>
  {% if user != person %}
    <div>
      <form action="" method="POST">
        {% csrf_token %}
        {% if user in followers %}
          <input type="submit" value="언팔로우">
        {% else %}
          <input type="submit" value="팔로우">
        {% endif %}
      </form>
    </div>
  {% endif %}
</div>
{% endwith %}
```

