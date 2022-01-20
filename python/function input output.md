# 함수

## 함수의 기초

> 함수 정의(선언) - 함수 호출

1. 이름(함수의 이름)
2. Input 이름
3. 로직
4. 결과

## return

> **return != print**

1. return이 없다면? **None**을 반환합니다.
2. return은 반드시 한 개만을 변환 (여러개로 보인다면, 튜플일 가능성 高)

## input

> parameter != Argument

1. Parameter: 함수를 실행할 때, 함수 내부에서 사용되는 식별자
2. Argument: 함수를 호출할 때, 넣어주는 값

```python
def function(ham): #파라미터: 햄
    return ham
function('spam')	#아규먼트: 스팸
```

- 필수 argument: 반드시 전달되어야 하는 것

- 선택 argument: 값을 전달하지 않아도 되는 경우 기본 값(기본 값을 설정한 경우)이 전달





Keyword argument, Positional argument

함수 호출

1. 정의된 순서대로 값을 넘겨줍니다. `add(1, 2)`

2. 정의된 이름(파라미터)을 키워드를 통해 넘겨줍니다. `add(x=1, y=2)`

   이 경우 한 개만 이름을 대입하는 건 불가능합니다. 한 개를 지정해서 입력했으면, 나머지도 마찬가지로 그렇게 입력해야 합니다. ~~add(x=1, 2), add(1, y=2)~~



함수 정의

1. 아무 것도 안하기

   ```python
   def add(x, y):
       pass
   ```

2. 기본 값 설정 (호출하는 입장에서 해당 파라미터는 선택적으로 쓸 것)

   ```python
   def add(x, y=0):
       pass
   ```

3. 정의되지 않은 갯수

   1. 나열 (시퀀스)

      ```python
      def add(*args):
          pass
      ```

   2. Key - Value

      ```python
      def func(**kwargs):
          pass
      ```

      

