# HTML

> 문서의 구조를 나타내는 태그!

# CSS 기본

스타일을 지정하기 위해서 필요한 것?

선택자

같은 속성이 다르게 지정되어 있으면 어떻게 선택할래?

선택자 우선순위 

태그가 복잡하게 되어 있는데 자식에게 상속되는 CSS가 있다! 

(이게 아니면 모두다 지정 해줘야함)

## 선택자

* `*` : 전체 선택자
* `태그명` : 요소 선택자
* **`.class` : class 선택자** 
  * **일반적인 스타일링시 사용되는 것**
* `#id` : id 선택자
* `div > .children` : 자식선택자
  * div 태그 바로 밑에 있는 children 클래스를 가진 것
* `div .baby` : 자손선택자
  * div 태그 하위 모든 baby 클래스를 가진 것
## 기본 선택자 우선순위

* `!important`
* 인라인 `style`
* `*` < `태그명` <`.class` < `#id`
  * 같은 점수인 경우에는 CSS가 나중에 선언된 것
## CSS 상속

* 상속 되는 속성 => style 상속 O
* 상속 되지 않는 속성 => box model, position 상속 X
# CSS Boxmodel

모든 요소가 네모네모!
* box model 구성요소
  * contents
  * padding
  * border
  * margin
* box model 너비 지정 (`box-sizing`)
  너비를 지정할건데 기준을 무엇으로 할것인지?
  * content-box (기본값)

* border-box