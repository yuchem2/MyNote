2001년에 NIST로부터 공개된 [[Block Cipher Symmetric Encryption]]을 사용하는 [[Block Cipher]]로, [[DES(Data Encryption Standard)]]를 대체하기 위해 등장하였다

## History
---
[[DES(Data Encryption Standard)]]는 이론상의 공격과 철저한 비밀 키 탐색 공격을 통해 추론될 수 있다는 것이 밝혀졌고, 새로운 표준 암호의 필요성이 등장했다. 

AES 개발 이전에 [[Triple DES]]도 존재해 이것을 사용할 수도 있었지만 [[Triple DES]]의 경우 느렸고, 블럭의 크기도 작은 문제가 있어 US NIST에서 1997년에 새로운 표준 암호를 위한 제안서를 받기로 결정하였다.

1998년 6월 15개의 지원이 있었고, 1999년 9월에 5개가 최종 후보로 선정되었다. 그 후 최종적으로 *Rijndael*이 2000년 8월에 AES로 채택되었다. 그 후 2001년에 [FIPS PUB 197]이라는 이름으로 표준이 공개되었다

### Requirements
1. 비밀 키 [[Block Cipher Symmetric Encryption]] 암호
2. 블럭 크기가 $128 \; bit$여야 하고, 암호 키는 $128, 192, 256\; bit$여야 한다
3. [[Triple DES]]보다 강력하고 빨라야 한다
4. 활용할 수 있는 기간이 20-30년
5. 모든 상세 정보와 디자인 컨셉을 제공해야 한다
6. [[C]] 혹은 [[Java]]로 구현

### Evaluation Criteria
+ Initial criteria
	+ security: effor to practically [[Cryptanalysis]]
	+ cost: computational
	+ algorithm & implementation characteristics
+ Final criteria
	+ general security
	+ software & hardware implementation ease
	+ implementation attacks
	+ flexibility(in en/decrypt, keying, other factors)

### AES Shortlist
+ after testing and evaluation, shortlist in Aug, 99
	+ MARS(미국, IBM): 복잡하지만 빠르고, 높은 보안성
	+ RC6(미국, RSA): 간단하고 빠름, 낮은 보안성
	+ Twoflish(미국, Counterpane Systems): 복잡하지만 빠르고, 높은 보안성
	+ Rijndael(벨기에, 대학): 명쾌하고, 빠르고 적당한 보안성
	+ Serpent(영국/덴마크/이스라엘, 대학): 느리지만 명쾌하고, 높은 보안성
+ then, subject to further analysis & comment
+ saw contrast between algorithms with
	+ 복잡한 적당한 round와 간단하고 많은 round
	+ 기존 암호를 정제하는 것과 새로운 암호

## AES Cipher - Rijndael
---
벨기에의 *Rijmen-Daemen*에 의해 설계된 암호로, $128, 192, 256 \; bit$길이의 비밀 키를 제공하고, 블럭 크기가 $128\; bit$이다.  키 길이에 따라 AES-128, AES-192, AES-256으로 불린다

[[Feistel Cipher]]와 유사하게 작동하며 데이터를 $4 \; bytes$의 4개의 그룹으로 나눠 다루며 각 라운드에서 전체 블록에 대해 연산이 수행된다. 

설계의 목표는 알려진 공격으로부터 저항성을 가지고, 대부분의 CPU에서 빠르고, 긴밀하게 작동하는 것과 간단하게 구현하는 것이었다. 

### Simple Sturcture
![[Pasted image 20231021192144.png | 600]]
<div align="center">AES Encrpytion Process</div>

1. 입력 평문($16 \; bytes)$은 각 $4 \;bytes$의 그룹으로 나눈다. 즉, 총 4개의 그룹으로 나눠진다. (column을 기준으로 정렬)
2. Inital transformation: XOR key material & incomplete last round
3. 암호화 과정은 동일한 작동을 하는 9, 11, 13개의 라운드와 다른 마지막 라운드로 구성한다. 
	+ byte subsitution: 1개의 s-box가 각 byte에 대해서 수행
	+ shift rows: groups/columns 단위로 permutation이 수행
	+ mix columns: $GF(2^8)$의 계산을 통해 substitution
	+ add round key: bitwise XOR state with the expanded key
4. 마지막 라운드에서는 mix column과정이 제외된 상태로 수행된다.
5. 모든 연산은 XOR 혹은 테이블을 통해 수행된다. 그러므로 매우 빠르고 효과적으로 작동한다

#### Parameters
| Names                                  | 128bits  | 192 bits | 256 bits |
| -------------------------------------- |:--------:|:--------:|:--------:|
| Key Size(words/bytes/bits)             | 4/16/128 | 6/24/192 | 8/32/256 |
| Plaintext Block Size(words/bytes/bits) | 4/16/128 | 4/16/128 | 4/16/128 |
| Number of Rounds                       |    10    |    12    |    14    |
| Round Key Size(words/bytes/bits)       | 4/16/128 | 4/16/128 | 4/16/128 |
| Expanded Key Size(words/bytes)         |  44/176  |  52/208  |  60/240  |
### Detailed Structure
![[Pasted image 20231021193750.png | 600]]
<div align="center">AES Encrpytion and Decrpytion</div>
