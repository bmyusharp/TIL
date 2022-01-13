# 1월 12일 16시 17분

git이란?

```git bash
git init
```

-> 생성되는 .git 폴더에 모든 버전이 저장될 것 (직접 들어가서 볼 일은 없다)

작업을 완료-add-커밋-commit-버전

(Working Directory)---(Repository *저장소)

(commit: 버전을 만들다) 

# git

> 분산 버전 관리 시스템 (DVCS)
>



## 1. git 저장소 만들기

```bash
$ git init
Initialized empty Git repository in F:/first/.git/
(master) $
```

- `.git` 폴더가 생성 -> 버전이 기록되는 저장소
  - 해당 폴더를 지우게 되면 모든 버전이 삭제되니 주의!
- `(master)`

```bash
$ touch a.txt
$ git add a.txt

$ git commit -m 'First commit'
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'kwak jong tak@DESKTOP-5K7IPSU.(none)')

$ git config --global user.email "1004romeo@naver.com"
$ git config --global user.name "bmyusharp"
$ git config --global -l
user.email=1004romeo@naver.com
user.name=bmyusharp
```

## 버전 기록하기

### add

```bash
$ git add 파일명
$ git add a.txt
$ git add my_folder/
$ git add a.txt b.txt
```

### status

```bash
$git status

# 커밋할 변경사항들 (Staging area)
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    b.txt

# 커밋을 위해 준비되지 않은 변경사항 (Staging area에 없음 X => Working directory)
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt

# 트래킹되지 않은 파일들 (Working directory)
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        c.txt
        

#위 둘(a와 c)을 나눠서 표시하는 이유는? -> a는 이미 커밋된 적이 있는 것, 반면 c는 처음 보인 것?
```

- 파일을 조작을 하는 것은 총 4가지 상황!
  - 생성 Create
  - ~~읽기 Read~~ git에서는 표시되지 않음?
  - 수정 Update
  - 삭제 Delete
  - CRUD





### commit

```bash
$ git commit -m '커밋메시지'
```

- 커밋 메시지는 항상 해당 변경(버전의 내용-변경사항-에 대해서 나타낼 수 있도록 기록한다.)

> 질문(앞으로도 나올 수 있음): 왜 add, commit 두 개의 명령어만 사용할까요?
>
> e.g. 2/14: 하루 종일 보고서, 발표자료 작업... 2/15: 보고서 수정 2/16: 보고서 수정 2222
>
> 1안) 작업 => 버전으로 기록
>
> 2안) 보고서를 하나의 버전으로 (2/14: v1: 보고서 v2: 발표자료)(2/15: 보고서 수정 2/16 보고서 수정 2222)
>
> 발표자료는 냅두고 보고서를 버전 되돌리기 가능
>
> 

```bash
kwak jong tak@DESKTOP-5K7IPSU MINGW64 /f/first (master)
$ touch b.txt


kwak jong tak@DESKTOP-5K7IPSU MINGW64 /f/first (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        b.txt

nothing added to commit but untracked files present (use "git add" to track)

$ git add b.txt
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   b.txt
```

git에 의해 하나하나 감시되고 있다.
### log

```bash
$ git log
commit 4013a6c70880fcc9387faf30f9e62a4cb7454b8b (HEAD -> master)
Author: bmyusharp <1004romeo@naver.com>
Date:   Wed Jan 12 16:39:03 2022 +0900

    First commit
```

### git에서 관리하는 파일 변경사항 상태

- untracked: 커밋에 포함된 적 없는 파일
- tracked
  - modified: 커밋에 비해서 수정된 경우
  - staged: 수정되었는데, 커밋 되기 전 목록에 추가되어 있는 경우
  - commited: 커밋 된 상태 (이후 변경사항이 없는 상태)
