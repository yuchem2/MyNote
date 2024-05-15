## 기본 매개변수 함수
```python
def hello(num, name = 'Yoon', a = 3, b = 1, c = 2):
	for i in range(num):
		print('hello %s' % (name), end = ' ')

hello(1) # hello Yoon
hello(2, 'Kim') # hello Kim Hello Kim
```
## 가변 매개 변수
```python
def add_para(*args):
	print(args)
	return sum(args)

print(add_para(3, 4, 5, 6)) # (3, 4, 5, 6), 18
print(add_para(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)) # (1, 2, 3, 4, 5, 6, 7, 8, 9, 10), 55

print("1", "2", "3") # 1 2 3

def show_para(**args): 
	print(args) 
	
show_para(a=1, b=2, c=3) # {'a': 1, 'b': 2, 'c': 3}

dic = {'name':'yoon','job':'student', 'hobby':'coding'} 
show_para(**dic) # {'name': 'yoon', 'job': 'student', 'hobby': 'coding'
```

## OOP
Object Oriented Programming은 기존의 프로그래밍 패러다임(Procedural Programming, 절차형 프로그래밍)과는 다른 새로운 패러다임
+ Procedural Programming
	+ 조건, 기능, 변수를 순서대로 정의하고 실행
	+ 조건, 기능이 증가할 때마다 함수와 변수 등 정의해야 할 요소들이 계속 증가해 복잡해질 수록 비효율적
+ Object Oriented Programming
	+ 객체와 캡슐화: 데이터를 캡슐로 싸서 외부의 접근으로부터 데이터를 보호하는 객체 지향 특성
	+ 상속성
	+ 다형성: 하나의 기능이 서로 다르게 보이거나 다르게 작동하도록 만드는 기능
	+ 소프트웨어의 생산성 향성
	+ 실세계에 대한 쉬운 모델링
## Class
Class는 일반적으로 데이터와 기능을 함께 묶는 방법을 제공한다. Python에서 기본적으로 제공하는 리스트, 딕셔너리, 튜플, 셋 등이 모두 Class.

Class는 크게 멤버(데이터)와 메소드(기능)으로 구성된다. 멤버는 그 class가 가지고 있는 정보를 의미하며, 메소드는 class의 멤버를 변경하거나, 보여주거나 하는 기능들을 말한다. (함수)

### 접근제어
+ `private`: 멤버, 메소드는 클래스에서만 접근 가능
	+ 속성 앞에 `__`을 이용해 표시
+ `public`: 어떤 클래스에서도 접근 가능
+ `protected`: 멤버, 메소드가 선언된 클래스와 상속받은 클래스에서만 접근 가능
	+ 속성 앞에 `_`을 이용해 표시
### 상속과 추상
+ 추상화: 여러 클래스에 중복되는 속성
+ 상속: 기본 클래스의 공통 기능을 물려받고, 다른 부분만 추가, 변경하는 것
	+ 기본 클래스 = 부모 클래스 = parent class = super class = base class
	+ 기능을 물려 받는 클래스 = 자식 클래스 = child class = sub class = derived class
## Example
```Python
class class_name:
	"""
	__init__()
	method...
	__del__()
	"""

	def function_name():
		...
```

```python
class Person: 
	total_num = 0
	
	def __init__(self, name, age):
		self.name = name
		self.age = age
		total_num += 1

	def getInformation(self):
		print("name: %s, age: %d" % (self.name, self.age))

	def work(self):
		print("work")


class Student(Person):
	def __init__(self, name, age, school_name):
		super(name, age)
		self.school_name = school_name 

	def getInformation(self):
		print("name: %s, age: %d, school: %s" % (self.name, self.age, self.school_name))

	def work(self):
		print("study")

p = Person('Yoon', 26)
p2 = Person('kim', 20)
s = Student('Yoon', 26, 'Korea Univ')

p.getInformation()
s.getInformation()

print(p.total_num, s.total_num)
```


```Python
p = Point() # p.x = 0, p.y = 0

class Point:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def setXY(self, x, y):
        self.x = x
        self.y = y

    def getX(self):
        return self.x

    def getY(self):
        return self.y

    def display(self):
        print(f"({self.x},{self.y})")
        # (x, y)

def add_point(src1, src2):
    x = src1.getX() + src2.getX()
    y = src1.getY() + src2.getY()
    dest = Point()
    dest.setXY(x, y)
    return dest

def increase_by(dest, src):
    if isinstance(src, Point):
        x = dest.getX() + src.getX()
        y = dest.getY() + src.getY()
    else:
        x = dest.getX() + src
        y = dest.getY() + src
    dest.setXY(x, y)

def is_equal(no1, no2):
    return no1.getX() == no2.getX() and no1.getY() == no2.getY()

def main():
    p1 = Point(2, 5) # p1.x = 2, p1.y=5
    p2 = Point(-3, 3) # p2.x = -3, p2.y = 3
    p3 = Point() # p3.x = 0, p3.y = 3

    p3 = add_point(p1, p2) # p3.x = -1 p3.y = 8
    print("p1 = ", end="")
    p1.display()
    print("p2 = ", end="")
    p2.display()
    print("p3 = ", end="")
    p3.display()
    print("------------------------------")

    # increase_by(p1, 5)  # 숫자로 증가시키는 경우
    increase_by(p1, p2)  # 다른 객체로 증가시키는 경우
    print("p1 = ", end="")
    p1.display()
    print("------------------------------")

    if is_equal(p1, p3):
        print("p1 == p3")
    else:
        print("p1 != p3")
    print("------------------------------")

if __name__ == "__main__":
    main()

```
