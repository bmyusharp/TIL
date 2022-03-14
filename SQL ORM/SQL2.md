## WHERE

```sqlite
CREATE TABLE users(
	first_name TEXT NOT NULL,
    last_name TEXT NOT NULL,
    age INTEGER NOT NULL,
    country TEXT NOT NULL,
    phone TEXT NOT NULL,
    balance INTEGER NOT NULL
); --실행, 테이블(users) 확인
```

쉘 켜기: *ster)* sqlite3 파일명.sqlite3

sqlite> .mode csv

sqlite> .import users.csv users

sqlite .tables



## SQLite Aggregate Functions

> 집계 함수

- 값 집합에 대한 계산을 수행하고 단일 값을 반환
  - 여러 행으로부터 하나의 결괏값을 반환하는 함수
- SELECT 구문에서만 사용됨
- 예시
  - 테이블 전체 행 수를 구하는 COUNT(*)
  - age 컬럼 전체 평균 값을 구하는 AVG(age)

`COUNT`, `AVG`, `MAX`, `MIN`, `SUM` 등...

- COUNT

  - users 테이블의 레코드 총 개수를 조회하려면?

    ```sqlite
    SELECT COUNT(*) FROM users;
    ```

- AVG, SUM, MIN, MAX

  - 위 함수들은 해당 (컬럼)이 숫자(INTEGER)일 때만 사용 가능

    ```sqlite
    SELECT AVG(age) FROM users WHERE age >= 30; 
    -- 30살 이상인 사람들의 나이 평균
    ```

    ```sqlite
    SELECT first_name, MAX(balance) FROM users; 
    -- 계좌 잔액이 가장 높은 사람
    ```

    ```sqlite
    SELECT AVG(balance) FROM users WHERE age >= 30; 
    -- 30살 이상인 사람들의 계좌 평균 잔액
    ```

    

## LIKE

> query data based on pattern matching

패턴 일치를 기반으로 데이터를 조회하는 방법

2개의 와일드카드를 제공

- **`%`** (percent sign)
  - 0개 이상의 문자, **이 자리에 문자열이 있을 수도 있고, 없을 수도 있다.**
- **`_`** (underscore)
  - 임의의 단일 문자, **반드시 이 자리에 한 개의 문자가 존재해야 한다.**

참고) wildcard character

- 파일을 지정할 때, 구체적인 이름 대신에 여러 파일을 동시에 지정할 목적으로 사용하는 특수 기호
  - **`*`**, **`?`** 등



e.g. users 테이블에서 나이가 20대인 사람만 조회한다면?

```sqlite
SELECT * FROM users WHERE age LIKE '2_';
-- 2로 시작하는 두 자릿수 값을 조회
```

e.g. users 테이블에서 지역번호가 02인 사람만 조회한다면?

```sqlite
SELECT * FROM users WHERE phone LIKE '02-%';
-- 02-로 시작하는 전화번호를 조회
-- 02% 대신 02-%를 이용하는 이유는, 023으로 시작하는 번호 같은 예외를 잡아내기 위해서
```



## ORDER BY

> sort a result set of a query

조회 결과 집합을 정렬

SELECT 문에 추가하여 사용

정렬 순서를 위한 2개의 키워드 제공

**`ASC`**: 오름차순(default) / **`DESC`**: 내림차순

특정 컬럼을 기준으로 데이터를 정렬해서 조회하기

```sqlite
SELECT * FROM 테이블 ORDER BY 컬럼1, 컬럼2, ... DESC; -- (또는 ASC)
```



e.g. users에서 나이 순으로 오름차순 정렬하여 상위 10개만 조회한다면?

```sqlite
SELECT * FROM users ORDER BY age ASC LIMIT 10; 
-- 상위 10개 = LIMIT 10
```



e.g. 나이순, 성 순으로 오름차순 정렬하여 상위 10개만 조회한다면?

```sqlite
SELECT * FROM users 
ORDER BY age, last_name ASC
LIMIT 10;
```

-> 먼저 나이순으로 정렬되고, 같은 나이 안에서 성 순으로 정렬되는 순서로 정렬됨.



e.g. 계좌 잔액 순으로 내림차순 정렬하여 해당 유저의 성과 이름을 10개만 조회한다면?

```sqlite
SELECT last_name, first_name 
-- 최종적으로 원하는 컬럼들은 보통 문제 마지막에 나옴(한글 서순으로 인하여)
FROM users 
ORDER BY balance DESC 
LIMIT 10;
```





## GROUP BY

> make a set of summary rows from a set of rows

- 행 집합에서 요약 행 집합을 만듦

- SELECT문의 optional 절

- 선택된 행 그룹을 하나 이상의 열 값으로 요약 행으로 만듦
- 문장에 **`WHERE`** 절이 포함된 경우, **반드시 WHERE 절 뒤에 작성**해야 함!

데이터를 요약하는 상황에 주로 사용



e.g. users에서 각 성(last_name)씨가 몇 명씩 있는지 조회한다면? 

(김씨 인원 수 n명, 이씨 인원 수 m명, 박씨 인원수 o명....)

```sqlite
SELECT last_name, COUNT(*) 
FROM users GROUP BY last_name;
-- 아예 다른 컬럼이 출력됨 (테이블이 새로 생긴 것은 아님.)
-- 출력할 때 COUNT(*)을 바꾸고 싶다면,
SELECT last_name, COUNT(*) AS name_count
FROM users GROUP BY last_name;
```



### 잠깐 복습

Q. title과 content라는 컬럼을 가진 articles라는 이름의 table을 새롭게 만들어보세요! 두 컬럼 모두 비어있으면 안되며, rowid를 사용합니다.

```sqlite
CREATE TABLE articles (
	title TEXT NOT NULL,
    context TEXT NOT NULL
);
```

Q. articles 테이블에 값을 추가해보세요!

```sqlite
INSERT INTO articles
VALUES ('1번제목', '1번내용');
```





## ALTER TABLE

3가지 기능

1. **table 이름 변경**
2. **테이블에 새로운 column 추가**
3. column 이름 수정 (3.25.0버전에 등장)
4. drop column (column 삭제, 3.35버전에 추가됨, 우리 버전: 3.38)
   - PK가 아니어야 함.

```sqlite
-- 테이블 이름 변경
ALTER TABLE 테이블이름
RENAME TO 새로운테이블이름;

-- 컬럼 이름 변경
ALTER TABLE 테이블이름
RENAME COLUMN 현재컬럼이름 TO 새컬럼이름;

-- 컬럼 추가
ALTER TABLE 테이블이름
ADD COLUMN 컬럼이름 데이터타입설정;

-- 컬럼
ALTER TABLE 테이블이름
DROP COLUMN 컬럼이름;
```



컬럼 추가 시, NOT NULL 붙혀서 할 때, Error가 발생한다.

기존 레코드에서 새로 추가한 필드에 대한 정보가 없기 때문이다.

이 때, NOT NULL 설정 없이 추가하게 하거나, 없는 정보에 기본 값(DEFAULT) 설정하는 것으로 해결 가능하다.

- **`DEFAULT`** 기본 값 추가하기

```sqlite
ALTER TABLE news
ADD COLUMN subtitle TEXT NOT NULL
DEFAULT '소제목'; 
-- 기존 레코드의 새 필드에는 기본값으로 '소제목'을 주겠다.
```







### 어떤 코드를 보든 이해할 수 있어야 합니다.

-- SQL 에서는...

SELECT * FROM articles_article;

-- ORM 에서는...

Article.object.all()



