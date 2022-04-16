[TOC]

# Git

## Undoing Things

> 되돌리기

### 1. 파일 내용을 수정 전으로 되돌리기

> "Unmodifying a Modified File"

- Modified 파일 되돌리기
- Working Directory에서 파일을 수정했다고 가정해봅시다.
  - 만약 이 파일의 수정 사항을 취소하고, 원래 모습대로 돌리려면 어떻게 해야 할까요?

```bash
$ git restore <파일명>
```

수정된 해당 파일을 이전 커밋으로 되돌리는 것 (덮어쓰기) - 수정한 내역이 날아가고 복원할 수 없음

### 2. 파일 상태를 Unstage로 되돌리기 (add 된 것을 되돌리기)

```bash
$ git rm --cached <파일명>
```

add 된 파일을 다시 unstage로 되돌리는 방법 (커밋이 없을 때)

```bash
$ git restore --staged <파일명>
```

(커밋이 있을 때) add된 파일을 다시 Working Directory로 내리는 것

### 3. 바로 직전 완료한 커밋 수정하기

> 만약 "A기능 완성" 커밋을 남겼는데, 필요한 파일 중 1개를 빼놓고 커밋했다면...

```bash
$ git commit --amend
```

- 상황 별 2가지 기능

  1. 커밋 메시지만 수정 (마지막으로 커밋하고 수정된 것이 없을 경우

     Vim 편집기가 열리면서 커밋 메시지를 수정할 수 있음

     'I'를 누르고, '-- 끼워넣기 --' 확인 - 커서 등장 - esc로 입력모드 종료 - 저장 후 종료(':wq' 엔터)

  2. 이전 커밋 덮어쓰기 (Staging Area에 새로운 파일을 add하고 이 명령어를 입력했을 경우)

- '--amend' 는 커밋을 고치는 작업, 마지막 커밋 작업에서 빠뜨린 것을 넣거나 변경하는 것을 새로운 커밋을 사용하지 않아도 되게 해 주는 것

> 앗 이 파일 넣는거 깜빡함. / 이전 커밋에 오타만 고쳤음. / 커밋명을 잘못 입력함.

[중요] 이렇게 --amend 옵션으로 커밋을 고치는 작업은, 추가로 작업한 일이 작다고 하더라도, **이전의 커밋을 완전히 새로 고쳐서 새 커밋으로 변경하는 것을 의미한다**. 이전의 커밋은 일어나지 않은 일이 되며, 히스토리에도 남지 않는다.





## Reset & Revert

- 공통점: 과거로 되돌린다
- 차이점: 과거로 되돌린다는 내용을 기록하는가 (커밋으로 남기는가)



### 1. git reset

> 업데이트를 했는데, 이전 버전이 더 좋다고 느낄 경우
>
> 예전 버전으로 돌아가고 싶을 땐?

```bash
$ git reset [옵션] <커밋 ID(해시값)>
```

- **시계를 과거로 돌리듯이** 특정 커밋 상태로 되돌아간다.
- 특정 커밋으로 되돌아 갔을 때, 해당 커밋 이후로 쌓은 커밋들 **전부 사라짐**

#### 옵션

1. `--soft`
   - 돌아가려는 커밋으로 되돌아가고,
   - 이후의 커밋된 파일들을 **staging area에 돌려놓음 (commit하기 이전 상태)**
   - 즉, 다시 커밋할 수 있음 (다시 미래로 갈 수 있다)
2. `--mixed`
   - 돌아가려는 커밋으로 되돌아가고,
   - 이후의 커밋된 파일들을 **working directory에 돌려놓음 (add하기 이전 상태)**
   - 즉, unstaged 된 상태로 남아있음 (아직도 미래로 갈 수 있음)
   - **기본값은 --mixed 이다.**
3. `--hard`
   - 돌아가려는 커밋으로 되돌아가고,
   - 이후의 커밋된 파일들(tracked) **모두 working directory에서 삭제**
   - 단, Untracked된 파일들은 그대로 Untracked된 상태로 남아 있음 (커밋한 적이 없으니 당연하다)

#### 참고

git reflog로 삭제한 커밋으로 돌아갈 수 있긴 합니다.

```bash
$ git reflog
<커밋해시값> 헤드@{} reset: moving to <커밋해시값>
...
<커밋해시값> 헤드@{} commit: added .....

$ git reset --hard <복구할 커밋 해시값>
```



### 2. git revert

> 커밋 내역이 사라진다는 reset의 큰 단점을 보완할 수 있습니다.
>
> 특히, 협업에서 커밋 내역의 차이로 인한 충돌을 방지할 수 있습니다.

```bash
$ git revert <해시값>
```

- **특정 사건을 없었던 일로 만드는 행위**로써, **이전 커밋을 취소한다는 새로운 커밋**을 만든다.
- 커밋 내역을 삭제(git reset) vs 새로운 커밋 내역(git revert)

vim 편집기가 나오면 저장 후 종료

![image-20220415102139348](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/image-20220415102139348.png)

커밋명은 "Revert <과거 커밋명>"이 기본값이 된다.

![revert_](https://raw.githubusercontent.com/bmyusharp/TIL-assets/master/img/revert_.png)



**[참고사항]**

```shell
# 공백을 통해 여러 커밋을 한꺼번에 되돌리기 가능
$ git revert 7f6c24c 006dc87 3551584

# 범위 지정을 통해 여러 커밋을 한꺼번에 되돌리기 가능
$ git revert 3551584..7f6c24c

# 커밋 메시지 작성을 위한 편집기를 열지 않음 (자동으로 커밋 완료)
$ git revert --no-edit 7f6c24c

# 자동으로 커밋하지 않고, Staging Area에만 올림 (이후, git commit으로 수동 커밋)
# 이 옵션은 여러 커밋을 revert 할 때 하나의 커밋으로 묶는게 가능
$ git revert --no-commit 7f6c24c
```



## Git workflow

브랜치Branch와 풀 리퀘스트Pull request를 이용한 협업

1) Feature Branch Workflow

   Shared repository model (저장소의 소유권이 **있는** 경우)

2) Forking Workflow

   Fork & Pull model (저장소의 소유권이 **없는** 경우)



### 1. Feature Branch Workflow

1. clone을 통해 저장소를 로컬에 복제

2. 기능 추가를 위해 branch 생성 및 기능 구현

3. 기능 구현 후 원격 저장소에 브랜치 반영

   ```bash
   $ git push origin feature/login
   # master로의 push가 아님..
   ```

4. branch들을 Pull Request 요청
5. 병합 완료-병합 완료 된 브랜치 삭제 (마스터 버전업)
6. 각 팀원들이 마스터 브랜치로 switch (pull받기 위해)
7. 버전업 된(병합된) master를 pull 받음
8. 병합 완료 된 로컬 브랜치 삭제

8까지가 한 싸이클



### 2. Forking Workflow

1. 소유권이 없는 원격 저장소를 fork를 통해 복제

2. 복제한 저장소에서 clone 받아옴

3. 추후 로컬 저장소를 원본 원격 저장소와 동기화하기 위해 URL을 연결

   ```bash
   $ git remote add upstream [원본 URL]
   # fork한 주소x 원본 저장소 주소. upstream은 다른 이름으로 해도 되지만 보통 upstream이다.
   ```

4. 기능 추가를 위해 branch 생성 및 기능 구현

5. 기능 구현 후 원격 저장소(fork한 내 저장소)에 브랜치 반영 (push)

6. 내 원격 저장소의 branch를 pull request

7. 원작자가 병합 완료(merge)하면, master 반영

8. 병합 완료된 브랜치 내 원격 저장소에서 삭제

9. 내가 마스터로 이동(switch)하고, git pull upstream master 하면...

9까지가 한 사이클