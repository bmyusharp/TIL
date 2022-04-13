# 깃 브랜치

git init

~~(master) <- 이게 무슨 뜻이냐면, 브랜치의 중심이 되는, master 브랜치입니다.

master브랜치는 '상용' (실제 서비스로서 사용되는)

다른 브랜치는 보통 별도의 독립적인 공간(버그 픽스를 한다던가..) (작업이 끝나면 브랜치가 master로 합쳐질 수 있다.)

## branch

다른 브랜치도 생성을 해 봅니다. 하나의 작업은 하나의 브랜치로 진행됩니다. 

조회, 생성, 삭제, 이동 등

`git branch`: 브랜치 목록 확인

`git branch <브랜치이름>`: 새로운 브랜치 생성

`git branch -d <브랜치이름>`: 특정 브랜치(병합된 것만) 삭제

``

**커밋했을 때 HEAD가 master에 있는 것 확인**

`git log --oneline`: 깃 로그가 한 커밋당 한 줄씩만 차지하여 출력됨

`git switch <브랜치이름>`: **다른 브랜치로 이동**

![image-20220311092412720](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311092412720.png)

**헤드**를 바꾸고 git log로 확인해보면 현재 브랜치까지의 로그를 볼 수 있음

`--all`: 전체 브랜치 로그 보기

`--graph`: 가지 모양으로 보기

![image-20220311093105994](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311093105994.png)

![image-20220311093344214](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311093344214.png)

(병합되지 않은 브랜치를 삭제하지 못함)(단, 강제로 삭제한다면 -D 사용)

`git branch -D <브랜치이름>`: 특정 브랜치 강제 삭제

`git switch -c <브랜치이름>`: 브랜치 생성+이동 동시에 하는 것(branch + switch)



**(주의!)** 브랜치 이동 후 새로운 파일을 만들어도 커밋하지 않으면(staging area에 들어가지 않는다면) 깃 안에 들어가지 않았으므로, 헤드 이동에 따라 변하지 않는다.



## merge (병합) 의 3가지 과정

`git merge <병합할 브랜치 이름>`: 브랜치 병합

- 주의점: merge하기 전에 일단 다른 브랜치를 합치려고 하는, 즉 메인 브랜치로  헤드를 이동(switch)해야 함

### 1. fast-forward

머지를 해도 새로운 커밋이 생기지 않는 경우

![image-20220311101128065](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311101128065.png)

그냥 최신 커밋이 된 것일 뿐

머지가 된 브랜치는 삭제 (-d)

### 2. 3-way merge (merge commit)

![image-20220311102220828](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311102220828.png)

둘의 분기점, 갈라진 브랜치, 원래 마스터 3가지를 사용하여 merge하는 것

![image-20220311102426361](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311102426361.png)

![image-20220311102434502](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311102434502.png)

![image-20220311102450760](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311102450760.png)

알아서 커밋명이 생성됨 (Merge branch 'signout')

머지가 끝났으니 signout 브랜치 삭제

### 3. merge conflict

- merge하는 두 브랜치에서 **같은 파일의 같은 부분**을 동시에 수정하고 merge하면, git은 해당 부분을 자동으로 merge해주지 못함

- 반면 동일 파일이더라도 **'서로 다른 부분'**을 수정했다면 conflict 없이 자동으로 merge commit 된다.

![image-20220311103348323](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311103348323.png)

![image-20220311103359308](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311103359308.png)

![image-20220311103607122](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311103607122.png)

두 개의 파일을 동시에 보여준다.

- Vim 에디터가 갑자기 나올 수도 있음

![image-20220311104101153](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220311104101153.png)

주피터노트북 처럼 (입력 모드, 명령 모드)가 있다. 

`esc`를 누르고 `:`를 누르고 `wq`엔터

결국 최종적으로는 3-way-merge같은 형태로 끝난다.



### git undoing

### github workflow

이 두 가지를 나중에 할 것



### checkout

브랜치 이동을 할 때 switch는 비교적 최근에 나온 명령어이다.

checkout은 switch처럼 헤드 이동 뿐만 아니라 restore(되돌리기) 기능 중 선택할 수가 있다.

즉, 기존 checkout의 두 가지 기능을 switch와 restore로 나눠놓았다고 볼 수 있다.



