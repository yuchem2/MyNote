
평문의 일반적인 특징이나 평문과 암호문 쌍들의 특징에 의존해 비밀키를 추론하는 [[Security attack]]

특정 평문이나 사용하고 있는 암호키를 추론하고자 하는 데 사용되는 방법이다

즉 특정한 [[암호화 알고리즘(Cypotographic algorithms)]]의 입력값이나 암호문에 대한 정보가 있을 때 시도할 수 있는 방법으로 각 유형에 따라 다음과 같이 정리 할 수 있다. 

|  Type of Attack   | Known to Crtpyanalyst                                                 | Descript                                                                            |
|:-----------------:| --------------------------------------------------------------------- |:----------------------------------------------------------------------------------- |
|  Ciphertext Only  | Enctyption algorithm & Ciphertext                                     | 정보가 매우 최소한으로 존재하기 때문에 공격이 매우 어렵다                           |
|  Known Plaintext  | + One or more plaintext-ciphertext pairs formed with the secret key   | Probable-word attack이라고도 하는데, 평문의 일부 부분이 예측 가능한 상태            |
| Chosen Plaintext  | 암호 분석가가 선택한 평문과 그에 상응하는 암호문이 암호키와 함께 생성 | 특정 평문을 선택해 그 시스템에 입력으로 넣어 나오는 암호문들의 패턴을 분석하는 방법 |
| Chosen Ciphertext | 암호 분석가가 선택한 암호문과 그에 상응하는 평문이 암호키와 함께 생성 | 일반적이지 않은 방법                                                                |
|    Chosen Text    | Chosen Plaintext + Chosen Ciphertext                                  | 일반적이지 않은 방법                                                                |

