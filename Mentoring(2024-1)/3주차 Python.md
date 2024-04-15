## 주의
Python에서는 C와 다르게 블록을 들여쓰기를 통해 판단합니다. `{}`이 없기 때문에 들여쓰기를 잘 수행해야 중첩처리 및 코드 작성을 잘 할 수 있습니다. 
## 조건문
### `if...else`
```python
if condition: 
	print()
	# statement
else:
	# statement
```
### `elif`
```python
if condition1: 
	# statement
elif condition2:
	# statement
else:
	# statement
```
## 반복문
### `for`
```python
for iterate in element_object:
	# statement

for i in range(10):
	# statement

words = ['cat', 'windows', 'defenestrate']
for w in words:
	# statement

users = {'Hans': 'active', 'Éléonore': 'inactive', '景太郎': 'active'}

for user, status in users.items():
# [('Hans', 'active'), ...]
	# statement
```
#### `range()`
```python
range(st, end, iterate)
range(10) # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
range(10, 0, -1) # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```
+ 숫자 리스트를 만들어주는 함수
+ `st`는 시작 숫자, `end`는 끝 숫자, `iterate`는 증가값
+ `st`를 생략하는 경우 0부터 시작. `iterate`의 기본값은 1
### `while`
```python
while condition:
	# statement
```
### 추가
`for`과 `while` 모두 `else`구문을 추가로 사용할 수 있습니다. (조건이 참이 아닌 경우 )

```python
i = 0
while i < 10:
	i++
else: 
	print(i)

for i in range(10):
	print(i)
else: 
	print(i+1)
```
## 자주 사용하는 키워드 및 함수
+ `not` ==  `!`
+ `and` == `&&`
+ `or` == `||`
+ `in`: lhs가 rhs안에 있는 지 확인해주는 키워드. 즉, rhs에는 일반적으로 여러 원소를 담고 있는 리스트, 튜플, 딕셔너리, 문자열, 집합이 사용된다.

```python
str1 = 'abcdefghi'
'a' in str1 # True
'z' in str2 # False
```
+ `map()`
```python
# 1 2 3 4 5 6 7 8 9 10
str1 = input() # '1 2 3 4 5 6 7 8 9 10'
str1 = str1.split() # ['1', '2', '3', '4', ..., '10']

for i in str1:
	i = int(i)
# [1, 2, 3, ... , 10]

str1 = map(int, str1) # [1, 2, 3, 4, ..., 10]
str1 = map(int, input().split())
```
+ `zip()`
```python
str1 = ['A', 'B', 'C', 'D', ... , 'Z']
str2 = ['apple', 'banana', 'cat', 'dog', ..., 'zibra']

for i in ragne(len(str1)):
	print(str1[i], str2[i])

for alpha, obj in zip(str1, str2):
	print(alpha, obj)
	alpha += obj
```


## 출처
+ https://wikidocs.net/book/1
+ https://devdocs.io/python/
