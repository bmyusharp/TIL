# 로그인/회원기능

## 로그인/로그아웃

### 로그인

> 사용자의 요청정보를 바탕으로 로그인
>
> 비밀번호가 맞는지 확인하는 프로세스

* `AuthenticationForm` : `forms.Form`

  ```python
  AuthenticationForm(request, request.POST)
  ```

* 로그인

  * 요청정보, 사용자정보

  ```python
  from django.contrib.auth import login as auth_login
  auth_login(request, form.get_user())
  ```

* 로그인 완료된 이후에 

  * `@login_required` => 요청 URL 로그인 완료 한 다음으로 넘어갈 원래 주소

  ```python
  redirect(request.GET.get('next') or 'articles:index')
  ```

* 로그인은 이미 로그인한 사람이 할 필요가 있을까? => 아니요!

  ```python
  if request.user.is_authenticated:
      return redirect('articles:index')
  ```

### 로그아웃

* 로그아웃 기능

  ```python
  from django.contrib.auth import logout as auth_logout
  auth_logout(request)
  ```

* 로그인은 로그인 한 사람만 로그아웃 할 수 있도록

  ```python
  if request.user.is_authenticated:
  ```

## 회원 관리

### 회원 가입(User Create)

* `UserCreationForm` : `forms.ModelForm`

#### 선택

* 로그인 되어 있으면 => 회원가입을 왜해?

  ```python
  if request.user.is_authenticated:
      return redirect('articles:index')
  
  ```

* 회원가입한 다음에 로그인 자동으로 해줄까?

  ```python
  if form.is_valid():
      user = form.save()
      auth_login(request, user)
      return redirect('articles:index')
  ```

  * 만약에 내가 회원가입하고 로그인을 직접 시키려면?

    ```python
    if form.is_valid():
        user = form.save()
        return redirect('accounts:login')
    ```

### 회원 삭제(User Delete)

* 회원 삭제하면 로그아웃도 시켜야함

* 회원 정보 어디서 가져와요?

  ```python
  request.user
  ```

### 회원 수정(User Update)

* `CustomUserChangeForm`

  * https://docs.djangoproject.com/en/4.0/ref/contrib/auth/#user-model

  * 직접 필드를 커스텀한다!

    ```python
    class CustomUserChangeForm(UserChangeForm):
        class Meta:
            model = get_user_model() # User
            fields = ('email', 'first_name', 'last_name',)
    ```

* 회원 정보는 어디서 가져온다?

  ```python
  request.user
  ```

### 비밀번호 수정

* `PasswordChangeForm` : `forms.Form`
  * 옛날 비밀번호가 맞는지 확인
  * 비밀번호 두개가 맞는지 확인

























