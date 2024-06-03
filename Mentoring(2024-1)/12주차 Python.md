## 파일 입출력

```python
def open(file, mode='r', buffering=None, encoding=None, erros=None, newline=None, closefd=True)
```

| mode  | description                   |
| ----- | ----------------------------- |
| `'r'` | 읽기 전용모드(default)              |
| `'w'` | 쓰기 전용모드.                      |
| `'x'` | 이미 파일이 있는 경우 실패. 없는 경우 생성     |
| `'a'` | 쓰기 전용모드로 열면서 파일의 맨 끝부터 데이터 입력 |
| `'b'` | 바이너리 모드                       |
| `'t'` | 텍스트 모드(default)               |
| `'+'` | 업데이트 모드(읽기/쓰기)                |

```python
f = open('./example1.txt', 'w')
for i in range(10):
	data = "%d번째 줄입니다.\n" % (i+1)
	f.write(data)
f.close()
```

```python
f = open('./example1.txt', 'r')
while True:
	line = f.readline()
	if not line: break
	print(line)
f.close()
```

```python
f = open('./example1.txt', 'r')
lines = f.readlines()
for line in lines:
	print(line)
f.close()
```

```python
f = open('./example1.txt', 'r')
data = f.read()
print(data)
f.close()
```

```python
f = open('./example1.txt', 'r')
for line in f:
	print(line)
f.close()
```

```python
f = open('./example.txt', 'a')
for i in range(11, 20):
	data = '%d번째 줄입니다.\n' % i
	f.write(data)
f.close()
```

```python
with open('./example1.txt', 'w') as f:
	f.write('Life is too short, you need python')
```
