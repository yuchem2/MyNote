## 유니코드
기존 ASCII code는 127개의 문자만 처리할 수 있어서 비영어권의 문자를 표현하는데 문제가 많았다. 이로 인해 서유럽에서는 ISO8859, 한국에서는 KSC5601 등을 만들기 시작했지만, 여러 나라에서 따로 문자 셋을 만들었기 때문에 *하나의 문서에 여러 나라의 언어를 동시에 표현할 수 있는 방법*이 없었다.

이런 문제를 해결하고자 하는 것이 Unicode. 모든 나라의 문자를 모두 포함하도록 4B 크기의 모든 문자를 할당하였다. 유니코드는 문자 외에 다양한 기호도 포함하고 있다.
### Python 유니코드
파이썬은 3.0.0부터 모든 문자열을 유니코드로 처리하고 있다. 파이썬에서 사용하던 모든 문자열은 유니코드 문자열.
#### Encode
유니코드 문자열은 인코딩 없이 파일에 적거나 다른 시스템으로 전송할 수 없다. 유니코드 문자열은 단순히 문자 셋의 규칙이기 때문.

파일에 적거나 다른 시스템으로 전송하기 위해서는 바이트 문자열로 변환해야 한다. 유니코드 문자열을 바이트 문자열로 바꾸는 것을 '인코딩'이라고 한다.

```python
a = 'Life is too short'
b = a.encode('utf-8')
print(b) # b'Life is too short'
type(b) # <class 'bytes'>

a = '한글'
a.encode('ascii') # error
a.encode('euc-kr') # b'\xc7\xd1\xb1\xdb'
a.encode('utf-8') # b'\xed\x95\x9c\xea\xb8\x80'
```
#### Decode
바이트 문자열을 유니코드 문자열로 바꾸는 행위를 말한다.
```python
a = '한글'
b = a.encode('euc-kr')
b.decode('euc-kr') # '한글'
b.decode('utf-8') # error
```

### 소스 코드 인코딩
```python
# -*- coding: utf-8 -*-
# -*- coding: euc-kr -*-
```
소스 코드도 파일이기 때문에 인코딩 타입이 필수. 소스 코드 가자 위에 위와 같은 문장을 추가한다면 입력한 인코딩 방법에 따라 인코딩이 된다. Python 3.0.0부터는 `utf-8`이 기본값이므로 `utf-8`로 인코딩한 소스 코드라면 생략해도 된다.
## Closure & Decorator
### Closure
Closure는 함수 안에 내부 함수(inner function)을 구현하고 그 내부 함수를 리턴하는 함수를 말한다. 

```python
def mul3(m):
	return m * 3;

def mul5(m):
	return m * 5;
```

```python
class Mul:
	def __init__(self, m):
		self.m = m

	def __call__(self, n): 
		return self.m * n;

mul3 = mul(3)
mul5 = mul(5)
print(mul3(10))
print(mul5(10))
```

```python
def mul(m):
	def wrapper(n):
		return m*n;
	return wrapper

mul3 = mul(3)
mul5 = mul(5)
print(mul3(10)) # 30
print(mul5(10)) # 50
```

`mul`함수는 호출 시 인수로 받은 `m` 값을 `wrapper` 함수에 저장하여 리턴한다. 이러한 형태로 자신의 내부 함수에 특정 인자를 저장한 상태로 리턴하는 함수를 closure라고 한다.
### Decorator
```python
import time

def myfunc():
	start = time.time()
	print("함수 실행");
	end = time.time()
	print("수행시간: %f초" % (end-start))

myfunc()
```

```python
import time

def elapsed(original_func):
	def wrapper():
		start = time.time()
		result = original_func()
		end = time.time()
		print("수행시간: %f초" % (end-start))
		return result
	return wrapper

def myfunc():
	print("함수 실행")

decorated_myfunc = elapsed(myfunc)
decorated_myfunc()
```

위와 같이 기존 함수를 바꾸지 않고 기능을 추가할 수 있는 `elapsed`와 같은 closure를 decorator라고 한다. 파이썬 decorator는 아래와 같은 방법으로도 적용이 가능하다.

```python
@elapsed
def myfunc():
	print("함수 실행")

myfunc()
```

`@func_name`을 함수 바로 위에 작성한다면 `func_name`을 decorator로 인식해 자동으로 적용하여 함수를 구성해 준다. 

위 예시의 경우 decorator는 기존 함수가 어떤 인수를 취할지 알 수 없기 때문에 `myfunc` 함수를 수정하는 경우 오류가 발생할 수 있다. 그래서 가변 매개변수 `*args`, `**kwargs`를 이용해 코드를 수정해 decorator를 작성하는 것이 바람직하다. 

```python
# decorator2.py
import time

def elapsed(original_func):   # 기존 합수를 인수로 받는다.
    def wrapper(*args, **kwargs):   # *args, **kwargs 매개변수 추가
        start = time.time()
        result = original_func(*args, **kwargs)  
        end = time.time()
        print("수행시간: %f초" % (end-start)) 
        return result  
    return wrapper

@elapsed
def myfunc(msg):
    """ 데코레이터 확인 함수 """
    print("출력: %s" % msg)

myfunc("You need python")
```

> `*args`는 모든 입력 인수를 튜플로 변환하는 매개변수, `**kwargs`는 모든 키=값 형태의 입력을 딕셔너리로 변환하는 매개변수
## Iterator & Generator
### Iterator
`next` 함수 호출 시 계속 그 다음 값을 리턴하는 객체. 한번 순회를 마치면, 더 이상 그 값을 가져오지 못하는 특징을 가지고 있다. 

```python
a = [1, 2, 3]
ia = iter(a)
type(ia) # <class 'list_iterator'>
next(ia) # 1
next(ia) # 2
next(ia) # 3
next(ia) # error

for i in iter(a):
	print(i)
```
#### Iterator 만들기
Class에 `__iter__`와 `__next__`를 이용하면 만들 수 있다.
+ `__iter__`: 반복 가능한 객체로 생성할 수 있도록 만들 수 있다. 구현하는 경우 `__next__`를 반드시 구현해야 한다.
+ `__next__`: 반복 가능한 객체의 값을 차례대로 반환하는 역할을 수행한다. 

```python
class MyItertor:
    def __init__(self, data):
        self.data = data
        self.position = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.position >= len(self.data):
            raise StopIteration
        result = self.data[self.position]
        self.position += 1
        return result

if __name__ == "__main__":
    i = MyItertor([1,2,3])
    for item in i:
        print(item)

```
### Generator
Iterator를 생성해 주는 함수이다. Generator로 생성한 객체는 iterator와 마찬가지로 `next` 함수 호출 시 그 값을 차례대로 얻을 수 있다. 
```python
def mygen():
	yield 'a'
	yield 'b'
	yield 'c'

g = mygen()
type(g) # <class 'generator'>
next(g) # 'a'
next(g) # 'b'
next(g) # 'c'
next(g) # error
```

```python
def mygen():
	for i in range(1, 1000):
		result = i * i;
		yield result
gen = mygen()
print(next(gen)) # 1
print(next(gen)) # 4
print(next(gen)) # 9

gen = (i * i for i in range(1, 1000)) # generator expression
print(next(gen)) # 1
print(next(gen)) # 4
print(next(gen)) # 9
```
#### Generator 활용하기
```python
import time

def longtime_job():
    print("job start")
    time.sleep(1)  # 1초 지연
    return "done"

list_job = [longtime_job() for i in range(5)]
print(list_job[0]) 
```

```
job start
job start
job start
job start
job start
done
```

```python
import time

def longtime_job():
    print("job start")
    time.sleep(1)
    return "done"

list_job = (longtime_job() for i in range(5))
print(next(list_job))
```

```
job start
done
```
### 차이점
+ Class를 이용해 iterator를 작성하면 좀 더 복잡한 행동을 구현할 수 있다. 
+ generator를 이용하면 간단하게 iterator를 만들 수 있다. 
$\therefore$ Iterator의 성격에 따라 클래스로 만들 것인지, 제너레이터로 만들 것인지 선택해야 한다. 
## Annotation
### 동적언어와 정적언어
+ 동적 언어: 프로그램 실행 중에 변수의 타입을 동적으로 바꿀 수 있는 언어
+ 정적 언어: 컴파일 타임에 변수의 타입이 결정되고, 실행 시간 동안 타입을 변경할 수 없는 언어

동적 언어는 타입에 자유로워 유연한 코딩이 가능하고 쉽고 빠르지만, 타입을 잘못 사용해 버그가 생길 확률이 높다.
### Annotation
파이썬은 동적 언어의 단점을 극복하기 위해 3.5버전부터 annotation 기능을 지원하기 시작했다. 적극적인 타입 체크가 아닌 type annotation(타입에 대한 힌트를 알려 주는 기능)만 지원한다.

```python
# sample.py
num: int = 1

def add(a: int, b: int) -> int:
	return a+b

print(add(3, 4.3)) # no error
```

### mypy
적극적인 타입 체크를 해주는 외부 라이브러리. `mypy`를 이용해 annotation이 사용된 파일을 체크하면 오류 체크를 해준다. 

```shell
C:\...>mypy sample.py
sample.py:6 error: Argument 2 to "add" has incompatible type "float"; expected "int"
Found 1 error in 1 file (checked 1 source file)
```

```python
# sample.py
num: int = 1

def add(a: int, b: int) -> int:
	return a+b

print(add(3, 4))
```

```shell
C:\...>mypy sample.py
Success: no issuses found in 1 source file
```