## 자주 사용하는 표준 모듈
### `datetime`
연, 월, 일로 날짜를 표현할 때 사용하는 모듈
```python
import datetime

day1 = datetime.date(2021, 12, 14) # seconds
day2 = datetime.date(2023, 4, 5)

print(day2-day1) # 477
# 0: monday, .... 6: sunday
print(day1.weekday()) # 1
```
### `time`
시간과 관련된 모듈
```python
import time

time.time() # 1970.01.01 00:00:00 기준으로 지난 초 리턴
time.localtime(time.time()) # 연, 월, 일, 시, 분, 초... 의 형태로 변환

# time.localtime이 리턴된 튜플 형태의 값을 인수로 받아서 알아보기 쉬운 상태로 리턴 
time.asctime(time.localtime(time.time())) 
# Fri Apr 28 20:50:20 2024
time.ctime() 
# time.asctime(time.localtime(time.time()))
time.strftime('formate..', time.localtime(time.time()))

time.sleep(1) # 1초 쉬기 

for i in range(10):
	print(i)
	time.sleep(1)
	
```
### `math`
수학 관련 모듈
```python
import math
math.gcd(60, 100, 80) # 최대 공약수
math.lcm(3, 5) # 최소 공배수
math.pi # 원주율
math.e # 자연상수
math.tua # tau 상수
math.trunc(10) # 버림
```
### `random`
난수를 발생시키는 모듈
```python
import random
import time
random.random() # [0, 1.0] 사이의 실수 난수 리턴
random.randiant(1, 10) # [1, 10] 사이의 정수 난수 리턴
random.choice([1, 2, 3]) # 입력받은 리스트에서 난수 리턴
random.sample([1, 2, 3], 3) # 입력받은 리스트의 항목을 섞음

random.seed(1234) # 시드값 설정
random.seed(time.time())
```
### `os`
환경 변수나 디렉터리, 파일 등의 OS 자원을 제어할 수 있게 해주는 모듈
```python
import os

os.environ # 환경변수 값 리턴, 딕셔너리 형태
os.environ['PATH'] # PATH 환경 변수 내용 
os.chdir('file path') # 입력받은 파일 경로로 현재 디렉터리 변경
os.getcwd() # 현재 디렉터리 리턴
os.system('command') # 시스템 명령어 호출
os.popen('command') # 시스템 명령어 결과를 파일 객체로 리턴
os.mkdir('name') # 디렉토리 생성
os.rmdir('name') # 디렉토리 삭제
os.remove('name') # 파일 삭제
os.rename('src', 'dst') # src 이름의 파일을 dst로 변경
```
### `sys`
python 인터프리터가 제공하는 변수와 함수를 직접 제어할 수 있는 모듈
```python
import sys
sys.stdin.readline() # input()
sys.stdout.write() # print()
```
## 외부 모듈 예시
### 딥러닝 관련
+ `pytorch`: https://pytorch.org/
+ `tensorflow`: https://www.tensorflow.org/?hl=ko
+ `Keras`: https://keras.io/
+ `numpy`: 대규모 다차원 배열 및 행렬 처리 
+ `scikit-learn`: 분류, 회귀 및 클러스터링 관련
+ `SciPy`: `numpy` 기반의 무료 오픈 소스. 대규모 데이터 셋에서 과학 및 기술 컴퓨팅을 수행할 수 있는 모듈
+ `pandas`: 데이터 분석에 사용되는 데이터 조작 및 분석 도구 제공
+ `matplotlib`: MATLAB 식의 인터페이스 및 데이터 시각화
+ `opencv`: 이미지 프로세싱, 얼굴 인식, 문자 인식 
### 웹관련
+ `Django`: https://www.djangoproject.com/
+ `Flask`: https://flask.palletsprojects.com/en/3.0.x/
+ `FastAPI`: https://fastapi.tiangolo.com/
+ `beautifulsoup4`: html, xml 문서 파싱/크롤링
+ `selenium`: 웹 동적 크롤링
+ `requests`: http 요청