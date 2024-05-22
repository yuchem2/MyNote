## 모듈
### 모듈 만들기
```python
'''
mod1.py
'''
def add(a, b): 
	return a + b

def sub(a, b):
	return a - b

if __name__ == "__main__":
	print(add(5, 10))
	print(sub(5, 10))
```
### 모듈 불러오기
#### 같은 디렉터리 모듈
```python
import mod1
print(add(5, 10))
print(sub(5, 10))
```

```python
from mod1 import add
print(add(5, 10))
```

```python
from mod1 import *
print(add(5, 10))
print(sub(10, 5))
```
#### 다른 디렉터리 모듈

```
\main
	mod1.py
	mod2.py
\sub
	example.py
```

자신이 만든 모듈을 다른 디렉토리의 python 파일에서 불러오기 위해서는 python shell에서 sys 모듈을 통해 설정을 해야 한다.
```python
'''
example.py
'''
# import mod1
```

```python
>> import sys
>> sys.path ## python 디렉터리가 설치되어있는 디렉터리 목록
>> sys.path.append('\main')
```

```shell
>> set PYTHONPATH=C:\example\mod
```

## 패키지
python에서 package는 관련 있는 모듈의 집합을 말한다. python 모듈을 계층적(디렉터리 구조)으로 관리할 수 있는 방법을 말한다.

```
game/
	__init__.py 
	sound/
		__init__.py
		echo.py
		wav.py
	graphic/
		__init__.py
		screen.py
		render.py
	play/
		__init__.py
		run.py
		test.py
```
### `__init__.py`
해당 디렉터리가 package의 일부임을 알려주는 역할을 수행한다. 패키지에 포함된 디렉터리에 `__init__.py` 파일이 없다면 패키지로 인식되지 않는다.
> python 3.3부터는 `__init__.py`가 없어도 패키지로 인식한다. 하지만 하위 버전 호환을 위해 생성하는 것이 좋다.

`__init__.py` 파일은 패키지와 관련된 설정이나 초기화 코드를 포함할 수 있다. 
```python
# /game/__init__.py
from .graphic.render import render_test
VERSION = 2.0

def print_version_info():
	print(f'the version of this game is {VERSION}')
```

```python
import game
game.print_vesrion_info()
```

### 패키지 다운로드
일반적으로 다른 디렉터리의 사용자 모듈을 가져오는 경우는 거의 없다. 그러나 python은 많은 사람들이 만들어 놓은 활용 가능한 좋은 모듈이 매우 많다. 

`pip`은 python 모듈 관리 프로그램으로, 사람들이 많이 올려놓은 모듈을 설치할 수 있다. 
+ 모듈 리스트: https://pypi.org/

```shell
python -V
pip -V

pip install '설치할 패키지'
pip list # 설치된 패키지 확인 가능
pip uninstall '삭제할 패키지'
```