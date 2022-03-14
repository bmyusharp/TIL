# 환경 연습

...

```sqlite
SELECT * FROM examples;
```

Run Query / Run Selected Query (블록으로 선택한 부분만)

-> 어떤 데이터베이스에서 실행할지에 대한 선택지가 나옴.

```sqlite
CREATE TABLE classmates ( -- 테이블 이름: classmates
	id INTEGER PRIMARY KEY, -- id가 INTEGER형식의 pk가 됨
    name TEXT -- name이라는 TEXT형식의 key 생성
); -- 스키마 지정
```



- 테이블 생성 및 삭제

  CREATE TABLE / DROP TABLE

- .schema 명령어: 스키마 출력 (보기 좋은 출력형태는 아님)

```sqlite
DROP TABLE classmates; -- 테이블 삭제
-- 왼쪽 익스플로러에서 리프레쉬(f5같은거) 누르면 반영 됨
```



테이블 생성 실습

- name-TEXT

- age-INT

- address-TEXT

```sqlite
CREATE TABLE
classmates (
	name TEXT,
    age INT,
    address TEXT
);
```





# CRUD

## CREATE

- INSERT
  - "insert a single row into a table" 데이터를 하나의 테이블에 삽입하겠다.
  - 테이블에 단일 행 삽입

​	**INSERT는 특정 테이블에 레코드(행)를 삽입(생성)!**

```sqlite
INSERT INTO 테이블이름 (컬럼1, 컬럼2, ...) VALUES (값1, 값2, ...);
```

```sqlite
INSERT INTO classmates (name, age) VALUES ('홍길동', 23);
```

**(쉘에서) SELECT를 사용해 확인 (`Read` 역할)**

```
sqlite> SELECT * FROM classmates;
```

![image-20220314100438280](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220314100438280.png)

우클릭-Run Query로도 확인 가능함

**모든 열에 데이터가 있을 경우 column을 명시하지 않아도 됨!**

```sqlite
INSERT INTO classmates VALUES ('홍길동', 23, '서울');
```

### 의문점

1. id는 어디로 갔을까? -> SQLite가 기본적으로 관리해주고 있었다 (**rowid**라는 이름으로) 따로 pk속성을 작성하지 않으면 값이 자동으로 증가하는 PK옵션을 가진 **rowid** 칼럼을 정의함

2. 비어있는 홍길동의 주소

   주소가 꼭 필요한 정보라면 공백으로 비워두면 안된다. (꼭 필요한 정보 설정 방법: **NOT NULL**)

   - 지우고 새로 만들기

```sqlite
DROP TABLE classmates;
CREATE TABLE
classmates (
	id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
);
```

`INT`, `INTEGER` 둘 다 같음. **단, PRIMARY KEY에서는 반드시 INTEGER라고 써야 함**

다시 insert로 '홍길동', 23, '서울' 입력 시 '4개의 요소를 필요로 하나 3개만 입력되었습니다.'라고 표시됨 **rowid가 아니라 직접 id로 pk를 정의했기 때문(자동으로 입력 안 됨)**

- 방법 1: id를 포함한 모든 value를 작성 *(1, '홍길동', 23, '서울')*

- 방법 2: 각 벨류에 맞는 컬럼들을 명시적으로 작성

```sqlite
INSERT INTO classmates (name, age, address) VALUES ('홍길동', 23, '서울');
```

*이번 실습에서는 rowid 사용해서 편하게 진행*



복수 행(여러 줄) 입력방법

```sqlite
INSERT INTO classmates 
VALUES
('홍길동', 30, '서울'), -- ,(콤마로 계속해서 입력가능하다는 것)
('김철수', 30, '대전'),
...
('최전자', 28, '부산'); -- 마지막은 ;(컬럼)임에 주의!
```



> 이런 건 될까? 저런 건 될까? 싶은 것들 직접 해 보시길 바랍니다.



## READ

**SELECT statement**

> query data from a table

SQLite에서 가장 복잡한 문임. 

테이블에서 데이터를 조회

다양한 절(clause)와 함께 사용

- ORDER BY, DISTINCT, WHERE, LIMIT, GROUP BY

`LIMIT`

- constrain the number of rows returned by a query
- 쿼리에서 반환되는 행 수를 제한
- 특정 행부터 시작해서 조회하기 위해 **OFFSET(부트스트랩에서도 나온 적 있음)**(=grid, =goto?) 키워드와 함께 사용하기도 함

`WHERE`

> specify the search condition for rows returned by the query

- if문과 유사
- 쿼리에서 반환된 행에 대한 특정 검색 조건을 지정

`SELECT DISTINCT`

> remove duplicate rows in the result set

- 조회 결과에서 중복 행을 제거
- DISTINCT 절은 **SELECT 키워드 바로 뒤에 작성**해야 함

`OFFSET`

- 동일 오브젝트 안에서 오브젝트 처음부터 주어진 요소나 지점까지의 변위차(위치 변화량을 나타내는 정수형
- 예시: `LIMIT 10 OFFSET 5` : 6번째 행부터 10개의 행을 출력





```sqlite
SELECT * FROM 테이블이름; --전체 테이블 조회
SELECT 컬럼1, 컬럼2, ... FROM 테이블이름; --특정 컬럼만 조회
```

e.g. classmates 테이블에서 id, name 컬럼 값만 조회하세요.

```sqlite
SELECT rowid, name FROM classmates;
```

![image-20220314103730185](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220314103730185.png)



- LIMIT

e.g. classmates 테이블에서 id, name 컬럼 값을 하나만 조회하세요.

```sqlite
SELECT rowid, name FROM classmates LIMIT 1;
```

![image-20220314103858217](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220314103858217.png)



- OFFSET

e.g. 특정 부분에서 원하는 수 만큼 데이터 조회하기

classmates 테이블에서 id, name 컬럼 값을 세 번째에 있는 하나만 조회하세요.

```sqlite
SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2; --0,1,2라서 세번째는 2
```



- WHERE

e.g. classmates 테이블에서 id, name 컬럼 값 중에 주소가 서울인 경우의 데이터를 조회하세요.

```sqlite
SELECT rowid, name FROM classmates WHERE address = '서울';
```



- DISTINCT

e.g. classmates 테이블에서 age값 전체를 중복없이 조회하세요.

```sqlite
SELECT DISTINCT age FROM classmates;
```

![image-20220314104718460](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220314104718460.png)



## DELETE

```sqlite
DELETE FROM 테이블이름 WHERE 조건;
```

그럼 어떤 기준으로 데이터를 삭제하면 좋을까?

**중복 불가능한(UNIQUE)값인 rowid를 기준으로 삭제하자!**



e.g. classmates 테이블에 id가 5인 레코드를 삭제하세요.

```sqlite
DELETE FROM classmates WHERE rowid = 5;
```

![image-20220314110554023](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220314110554023.png)

- 삭제하고 나서

​	데이터 삭제는 잘 되었는데, 지워진 id는 어떻게 되는걸까?

​	데이터를 다시 추가해서 확인해보자

```sqlite
INSERT INTO classmates VALUES ('최전자', 28, '부산');
SELECT rowid, * FROM classmates;
```

![image-20220314110821550](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220314110821550.png)

​	5를 재활용했다... (Django Model때와는 다름)

**SQLite는 기본적으로 id를 재사용**

재사용 없이 사용하지 않은 다음 행 값을 사용하게 하려면?



- AUTOINCREMENT: Colum attribute

**SQLite가 사용되지 않은 값이나 이전에 삭제된 행의 값을 재사용하는 것을 방지**

Django에서는 이 속성이 중요하다고 생각해 기본 값으로 사용된 것이다.

테이블을 생성하는 단계에서 AUTOINCREMENT를 통해 설정 가능




## UPDATE

- UPDATE

> update data of existing rows in the table

기존 행의 데이터를 수정

SET clause에서 테이블의 각 열에 대해 새로운 값을 설정



**조건(`WHERE`)**을 통해 특정 레코드 수정하기

**중복 불가능한(UNIQUE) 값인 rowid를 기준으로 수정하자!**



e.g. classmates 테이블에 id가 5인 레코드를 수정하세요. 이름을 홍길동으로, 주소를 제주도로 바꿔주세요!

```sqlite
UPDATE classmates
SET name='홍길동', address='제주도'
WHERE rowid = 5;
--가독성을 위해 줄바꿈하여 입력함
SELECT rowid, * FROM classmates; --조회
```

![image-20220314112507442](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220314112507442.png)





## CRUD 정리하기

|      |  구문  |                             예시                             |
| :--: | :----: | :----------------------------------------------------------: |
|  C   | INSERT | INSERT INTO 테이블이름 (컬럼1, 컬럼2, ...)  VALUES (값1, 값2); |
|  R   | SELECT |             SELECT * FROM 테이블이름 WHERE 조건;             |
|  U   | UPDATE |    UPDATE 테이블이름 SET 컬럼1=값1, 컬럼2=값2 WHERE 조건;    |
|  D   | DELETE |              DELETE FROM 테이블이름 WHERE 조건;              |

