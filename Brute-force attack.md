
공격자가 평문으로 이해할 수 있는 번역이 얻어질 때가지 암호문에 가능한 모든 키를 시도해 키를 추론하는 [[Security attack]]

평균적으로 모든 가능한 키의 절반을 시도한다면 성공에 도달할 수 있다

대부분의 일반적인 공격이 성공하는 시간은 키의 길이에 의존적이다.

|      Key size (bits)      | Number of Alternavite Keys     | Time required at 1 decryption/$\micro s$              | Time required at $10^6$ decryption  $\micro s$ |
|:-------------------------:| ------------------------------ | ----------------------------------------------------- | ---------------------------------------------- |
|            32             | $2^{32} = 4.3 \times 10^9$     | $2^{32} \micro s = 35.8$ minutes                      | $2.15$ milliseconds                            |
|            56             | $2^{56} = 7.2 \times 10^{16}$  | $2^55 \micro s = 1142$ years                          | $10.1$ hours                                   |
|            128            | $2^{128} = 3.4 \times 10^{38}$ | $2^{127} \micro s = 5.4 \times 10^{24}$ years         | $5.4 \times 10^{18}$ years                     |
|            168            | $2^{168} = 3.7 \times 10^{50}$ | $2^{167} \micro s = 5.9 \times 10^{36}$ years         | $5.4 \times 10^{30}$ years                     |
| 26 charcters (permutation) | $26! = 4 \times 10^{26}$       | $2\times 10^{26} \micro s = 6.4 \times 10^{12}$ years | $6.4 \times 10^6$ years                        |

