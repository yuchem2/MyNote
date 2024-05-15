간단한 클래스 예시 문제:

## 사각형 클래스 만들기
- 사각형 클래스를 정의하고, 가로와 세로 길이를 속성으로 갖습니다.
- 사각형의 넓이를 계산하는 메서드를 포함해야 합니다.
```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def calculate_area(self):
        return self.width * self.height

# 테스트
rectangle = Rectangle(5, 4)
print("넓이:", rectangle.calculate_area())  # 출력: 20
```

## 문제: 은행 계좌 관리 시스템

1. Account 클래스 작성하기:
    - Account 클래스를 작성합니다. 이 클래스는 다음과 같은 속성을 가져야 합니다:
        - account_number (계좌 번호)
        - account_holder (예금주 이름)
        - balance (잔액)
    - 다음과 같은 메서드를 포함해야 합니다:
        - deposit(amount): amount 만큼의 금액을 계좌에 입금합니다.
        - withdraw(amount): amount 만큼의 금액을 계좌에서 출금합니다. 단, 출금 금액이 잔액을 초과하는 경우에는 출금이 이루어지지 않아야 합니다.
2. SavingsAccount 클래스 작성하기:
    - Account 클래스를 상속받는 SavingsAccount 클래스를 작성합니다.
    - SavingsAccount 클래스는 다음과 같은 속성을 추가로 가져야 합니다:
        - interest_rate (이자율)
    - 이자율은 객체 생성 시에 초기화되어야 합니다.
    - 이자율에 따라 매월 잔액에 이자가 추가되어야 합니다. (이자율 * 잔액)
3. CurrentAccount 클래스 작성하기:
    - Account 클래스를 상속받는 CurrentAccount 클래스를 작성합니다.
    - CurrentAccount 클래스는 다음과 같은 속성을 추가로 가져야 합니다:
        - overdraft_limit (대출 한도)
    - 계좌의 잔액이 0 미만이 될 경우에도 overdraft_limit 만큼의 금액을 빌릴 수 있어야 합니다. 단, 이때는 이자가 발생하지 않습니다.

```python
class Account:
    def __init__(self, account_number, account_holder, balance):
        self.account_number = account_number
        self.account_holder = account_holder
        self.balance = balance
    
    def deposit(self, amount):
        self.balance += amount
        print(f"{amount}원이 입금되었습니다. 현재 잔액: {self.balance}원")
    
    def withdraw(self, amount):
        if self.balance >= amount:
            self.balance -= amount
            print(f"{amount}원이 출금되었습니다. 현재 잔액: {self.balance}원")
        else:
            print("잔액이 부족합니다. 출금이 취소됩니다.")

class SavingsAccount(Account):
    def __init__(self, account_number, account_holder, balance, interest_rate):
        super().__init__(account_number, account_holder, balance)
        self.interest_rate = interest_rate
    
    def add_interest(self):
        interest = self.balance * self.interest_rate
        self.balance += interest
        print(f"이자 {interest}원이 추가되었습니다. 현재 잔액: {self.balance}원")

class CurrentAccount(Account):
    def __init__(self, account_number, account_holder, balance, overdraft_limit):
        super().__init__(account_number, account_holder, balance)
        self.overdraft_limit = overdraft_limit
    
    def withdraw(self, amount):
        if self.balance >= amount or amount <= self.overdraft_limit:
            self.balance -= amount
            print(f"{amount}원이 출금되었습니다. 현재 잔액: {self.balance}원")
        else:
            print("잔액이 부족합니다. 대출 한도를 초과하여 출금이 취소됩니다.")

# 테스트
# SavingsAccount 테스트
savings_account = SavingsAccount("123456", "John", 1000, 0.05)
savings_account.deposit(500)
savings_account.add_interest()
savings_account.withdraw(2000)

# CurrentAccount 테스트
current_account = CurrentAccount("654321", "Alice", 2000, 1000)
current_account.withdraw(3000)
current_account.withdraw(1500)

```