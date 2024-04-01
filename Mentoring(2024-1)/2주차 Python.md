# 기본
## `print()`
```python
print(*objects, sep=' ', end='\n', flie=None, flush=False)
```
`file`에 입력된 `objects`를 `sep`로 나눠서 출력한다. 
## `input()`
`stdin`에서 한 줄 단위로 문자열을 읽어서 리턴 한다. 
# 자료형
기본적으로 python에서는 데이터 타입으로 변수를 지정하지 않는다. 각 리터럴(초기화 값)의 형태에 따라 자료형이 자동으로 결정된다.
## 숫자형
### 정수형
+ 양의 정수, 0, 음의 정수
+ 2, 8, 10, 16진수로 표현 가능. 진수 변환은 `int(x, base=10)`을 통해서도 할 수 있고, `bin(), oct(), hex()`로도 가능
	+ 2진수: `0b(0B)`
	+ 8진수: `0o(0O)`
	+ 16진수: `0x(0X)`
### 실수형
지수 표현, 부동소수점 표현 모두 가능
### 복소수형
`x+yj` 형태로 표기된다. `x`는 실수부, `y`는 허수부분이며 따로 분리가 가능하고, 연산도 따로 가능하다.
```python
a = 1.2 + 3.4j
b = 5.6 - 7.8j

print(type(a)) # <class 'complex'>
```
## 문자열
`"` 또는 `'`을 이용해 문자열을 정의한다. C와 다르게 문자열과 문자의 차이가 존재하지 않는다.

`"""` 또는 `'''`을 이용해 여러 줄 문자를 저장할 수 있다.
```python
multiline = '''
	... Life is too short 
	... You need python 
	... '''
```
### 연산
+ `+`를 통해서 두 문자열을 연결할 수 있음
+ `*`를 통해서 같은 문자열을 반복시켜 저장할 수 있다. `"python" * 2 = "pythonpython"`
+ `len()`을 통해 길이를 측정할 수 있음
### 인덱싱
문자열은 배열의 형태로 저장된다.
```python
a = "Life is too short, You need Python"
```

![[Pasted image 20240401220334.png]]
위 번호를 통해 각 문자를 직접 접근할 수 있음
```python
a[3] # 'e'
a[-3] # 'h'
a[-0] # 'L'
```

But, 문자열은 immutable 자료형이기 때문에 인덱싱을 직접 접근해 하나의 문자만 변경은 불가능하다
```python
simple = "Pithon"
a[1] # 'i'
a[1] = 'y' # error
```
### 슬라이싱
```python
a[0:4] # 'Life'
a[:4] # 'Life'
a[:] # 'Life is too short, You need Python'
a[19:] # 'You need Python'
a[19:-7] # == a[19] ~ a[-8] 'You need'
```
### formating
`%`를 통해 C처럼 서식 시정자를 사용할 수 있다. <a href="https://blockdmask.tistory.com/428">참고</a>
```python
print('my name is %s. age : %d' % ('blockdmask', 100))
age = 80
money = 20000
name = 'Kim'
weight = 80.12
etc = 'abcde'
print(s2 = 'my name is %s, age : %d, weight : %f, money : %d, etc : %s' % (name, age, weight, money, etc))
```

`format()`을 이용해 문자열을 다뤄 출력할 수 있다. <a href="https://blockdmask.tistory.com/424">참고</a>
```python
number = 12345
print("{0:,}".format(number))
print("{num}: {age}".format(num=number, age=20))
print('name : {}, city : {}'.format('BlockDMask', 'seoul'))
print('test1 : {0}, test2 : {1}, test3 : {0}'.format('인덱스0','인덱스1'))
print('this is {0:<10} | done {1:<5} |'.format('left', 'a'))
print('this is {0:>10} | done {1:>5} |'.format('right', 'b'))
print('this is {0:^10} | done {1:^5} |'.format('center', 'c'))
```

더 많은 정보는 <a href="https://devdocs.io/python~3.12/library/string">여기</a>를 참고
## 리스트
리스트는 C에서의 배열과 동일한 리터럴을 가진다. `[]`과 `,`을 통해 요소를 나열한 형태로 정의하면 된다.
```python
a = []
b = [1, 2, 3]
c = ['Life', 'is', 'too', 'short']
d = [1, 2, 'Life', 'is']
e = [1, 2, ['Life', 'is']]
```

문자열과 동일한 연산이 모두 가능하다. (더하기, 곱하기, 슬라이싱, 인덱싱, `len()`)
또한, mutable 자료형이기 때문에 수정 삭제도 가능하다
```python
a = [1, 2, 3]
a[2] = 4
a # [1, 2, 4]
del a[1] 
a # [1, 3]
```
### method
+ `append(x)`: 리스트 맨 마지막에 `x`를 추가
+ `sort()`: 리스트 요소를 순서대로 정렬
+ `reverse()`: 리스트 요소를 역순으로 뒤집어줌
+ `index(x)`: 리스트에 `x` 값이 있으면 인덱스 값을 리턴한다.
+ `insert(a, b)`: 리스트에 `a`번째 위치에 `b`를 삽입
+ `remove(x)`: 리스트에서 처음으로 등장하는 `x`를 삭제
+ `pop()`: 리스트 맨 마지막 요소를 리턴하고 그 요소는 삭제한다
+ `count(x)` 리스트에 `x`가 몇 개 있는 지 확인해 그 개수를 리턴
+ `extend(x)` `x`는 리스트 요소로, 원래 리스트에 `x`리스트를 더해준다. 즉 리스트간의 더하기 연산을 수행해준다.

더 많은 `method`는 <a href="https://devdocs.io/python~3.12/library/stdtypes#list">여기</a>를 참고
## 튜플
리스트와 유사하며 아래 나열한 부분에서만 차이가 존재한다.
+ `()`를 이용해 요소를 나열한다.
+ immutable 자료형으로 수정, 변이 불가능하다

문자열과 동일한 연산이 모두 가능하다. (더하기, 곱하기, 슬라이싱, 인덱싱, `len()`)

## 딕셔너리
딕셔너리는 `key`와 `value`로 구성되는 자료형으로, 연관 배열 혹은 해시라고도 한다. `value`는 중복이 허용되지만, `key`는 고유한 값이므로, 같은 값이 존재하는 경우 나중에 추가된 쌍만 유지된다. 
```python
{Key1: Value1, Key2: Value2, Key3: Value3, ...}
dic = {'name': 'pey', 'phone': '010-9999-1234', 'birth': '1118'}
```

| key   | value         |
| ----- | ------------- |
| name  | pey           |
| phone | 010-9999-1234 |
| birth | 1118          |
### 쌍 추가, 삭제하기
```python
a = {1: 'a'}
a[2] = 'b'
a # {1: 'a', 2: 'b'}
a['name'] = 'pey'
a # {1: 'a', 2: 'b', 'name': 'pey'}
a[3] = [1, 2, 3]
a # {1: 'a', 2: 'b', 'name': 'pey', 3: [1, 2, 3]}
del a[1]
a # {2: 'b', 'name': 'pey', 3: [1, 2, 3]}
```
### method
+ `keys()`: 딕셔너리의 `key`로 구성된 `dict_keys()` 객체를 리턴한다.
+ `values()`: 딕셔너리의 `value`로 구성된 `dict_values()` 객체를 리턴한다.
+ `items()`: 딕셔너리의 `key`와  `value` 쌍을 튜플로 묶은 `dict_items()` 객체를 리턴한다.
+ `clear()`: 딕셔너리 비우기
+ `get(key)`: `key`를 통해 `value`를 얻을 수 있다. 만약 대응되는 키가 없는 경우 `None`을 리턴한다. 만약 `get(key, x)`의 형태로 사용할 때 대응되는 키가 없는 경우 `x`를 리턴한다.
+ `in`을 통해 `key`가 딕셔너리에 존재하는 지 확인할 수 있다. `'nane' in a`

더 많은 `method`는 <a href="https://devdocs.io/python~3.12/library/stdtypes#dict">여기</a>를 참고
## 집합
수학 집합에 관련된 연산을 수행하기 위한 자료형으로 다음과 같은 특징이 있다.
+ 중복을 허용하지 않는다
+ 순서가 없다.

```python
s = set()
s1 = set([1, 2, 3])
s1 # {1, 2, 3}
s2 = set("Hello")
s2 # {'e', 'H', 'l', 'o'}
```
### 교집합, 합집합, 차집합
```python
s1 = set([1, 2, 3, 4, 5, 6])
s2 = set([4, 5, 6, 7, 8, 9])
```
#### 교집합
```python
s1 & s2 # {4, 5, 6}
s1.intersection(s2) # {4, 5, 6}
```
#### 합집합
```python
s1 | s2 # {1, 2, 3, 4, 5, 6, 7, 8, 9}
s1.union(s2) # {1, 2, 3, 4, 5, 6, 7, 8, 9}
```
#### 차집합
```python
s1 - s2 # {1, 2, 3}
s2 - s1 # {8, 9, 7}
s1.difference(s2) # {1, 2, 3}
s2.difference(s1) # {8, 9, 7}
```
### method
+ `add(x)`: `x` 값을 추가한다.
+ `update()`: 여러 값을 한번에 추가한다.
+ `remove(x)`: `x` 값을 제거한다.

더 많은 `method`는 <a href="https://devdocs.io/python~3.12/library/stdtypes#set">여기</a>를 참고
# 출처
+ https://wikidocs.net/book/1
+ https://devdocs.io/python/