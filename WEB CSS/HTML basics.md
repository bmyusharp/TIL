# HTML 기본

HTML: Hyper Text Markup Language

Hyper Text는 참조(하이퍼링크)를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있게 하는 텍스트이다.

Markup Language는 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어이다.

```html
<!DOCTYPE html>
<html lang="en">
    <head>
    </head>
    <body>
    </body>
</html>
```

- HTML

```markdown
# HTML 기본
> 웹 페이지의 모습을 기술하기 위한 규약. 프로그래밍 언어가 아닌 마크업 언어이다.
```

- 마크다운

![image-20220203172741713](C:\Users\1004r\AppData\Roaming\Typora\typora-user-images\image-20220203172741713.png)

즉, **웹 페이지를 작성(구조화)하기 위한 언어**라고 할 수 있다.



## HTML의 기본 구조

- html: 문서의 최상위(root) 요소

- head: 문서 메타데이터 요소
  - 문서 제목, 인코딩, 스타일, 외부 파일 로딩 등. 일반적으로 브라우저에 **나타나지 않음**
- body: 문서 본문 요소
  - 실제 화면 구성과 관련된 내용

### head 예시

- title: 브라우저 상단 타이틀(<title>)

- meta: 문서 레벨 메타데이터 요소(<meta>)
- link: 외부 리소스 연결 요소 (CSS파일, favicon 등)(<link>)
- script: 스크립트 요소 (Javascript 파일/코드)(<script>)
- style: CSS 직접 작성 (<style>)

### 요소(element)

> <h1>contents</h1>

>  <여는 태그>내용</닫는 태그>

*HTML의 요소는 태그와 내용(contents)로 구성되어 있다.*

내용이 없는 태그들도 있음 (br, hr, img, input, link, meta)

요소는 중첩될 수 있다.

- 요소의 중첩을 통해 하나의 문서를 구조화
- 여는 태그와 닫는 태그의 쌍을 잘 확인해야 함
  - 오류를 반환하는 것이 아닌 그냥 레이아웃이 깨진 상태로 출력되기 때문에, 디버깅이 힘들어질 수 있음

#### 속성(attribute)

<a herf(속성명)="https://주소"></a>

태그별로 사용할 수 있는 속성은 다르다.

= 좌우로 공백x	쌍따옴표 사용(홀따옴표x) <- 속성 지정 스타일 가이드



