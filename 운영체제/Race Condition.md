여러 [[Process]]들이 같은 데이터에 동시에 접근하여 조작할 수 있고, 이 경우 실행 결과 값은 접근이 일어나는 특정 순서에 따라 달라지는 것을 말한다. 

이를 예방하기 위해서는 한번에 오직 하나의 [[Process]]만이 공유된 데이터를 조작하는 것을 보장할 필요가 있다. 즉, [[Process Synchronization]]이 필요하다. 
## Example
---
[[Producer-Consumer Problem]]에서 producer와 consumer 작업이 올바르게 분리되었다고 해도 그 작업이 **동시에** 실행된 경우 제대로 동작하지 않을 수 있음

e.g. counter의 값이 5이고, producer와 consumer가 동시에 counter++과 counter--를 수행한다면, 실행 순서에 따라 4, 5, 6이 될 수 있다. counter++과 counter-- 명령어는 다음과 같이 구현 될 수 있기 때문이다. 
+ counter++
``` 
register1 = counter
register1 = register1 + 1
counter = register1
```
+ counter--
```
register2 = counter
register2 = register2 - 1
counter = register2
```

위 경우에서 counter++과 counter--는 ATOMIC이 아니기 때문에 발생할 수 있는 오류이다. 여기서 ATOMIC이란 단일 박스에서 수행되지 않는 다는 것을 의미하고, 위 경우에서는 counter가 E-box로 이동한 후 연산이 수행되고, S-Box에 결과가 이동하여 저장된다. 