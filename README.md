# clean-code-python

[![Build Status](https://travis-ci.com/zedr/clean-code-python.svg?branch=master)](https://travis-ci.com/zedr/clean-code-python)
[![](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/download/releases/3.8.3/)


<br><br>

## Author

[@zedr](https://github.com/zedr), Thank you for making a great document!!!

<br><br>

## 목차

1. [소개](#소개)
2. [변수](#변수)
3. [함수](#함수)
4. [주석](#주석)
5. [형식맞추기](#형식맞추기)
6. [클래스 (객체지향 5원칙)](#클래스)
    1. [S: 단일 책임 원칙 (Single Responsibility Principle; SRP)](#단일-책임-원칙-single-responsibility-principle-srp)
    2. [O: 개방/폐쇄 원칙 (Open/Closed Principle; OCP)](#개방폐쇄-원칙-openclosed-principle-ocp)
    3. [L: 리스코프 치환 원칙 (Liskov Substitution Principle; LSP)](#리스코프-치환-원칙-liskov-substitution-principle-lsp)
    4. [I: 인터페이스 분리 원칙 (Interface Segregation Principle; ISP)](#인터페이스-분리-원칙-interface-segregation-principle-isp)
    5. [D: 의존성 역전 원칙 (Dependency Inversion Principle; DIP)](#의존성-역전-원칙-dependency-inversion-principle-dip)
7. [반복은 지양합시다. (Don't repeat yourself; DRY)](#반복은-지양합시다-dont-repeat-yourself-dry)
8. [Translations](#translations)

<br><br>

## 소개

Robert C. Martin의 책, [*Clean
Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)을 참고하였습니다.

<br>

이 문서는 Python에 맞게 수정되었으며 style guide가 아닙니다. 

이 문서는 Python으로 읽을 수 있고(readable) 재사용 가능하며(reusable), 리펙토링 가능한(refactorable) 소프트웨어를 만들어 내기 위한 가이드라인을 제시합니다.

<br>

이 문서의 모든 것을 완전히 따를 필요는 없으며, 각 구성원 간 보편적 합의에 따라가면 됩니다.

다시 말하지만 이 문서에서 언급하는 것들은 모두 지침일 뿐입니다.   
다만, *Clean Code*의 저자들에 의해 수년간의 경험에 의해 정립된 것들입니다.

<br>

[clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)의 문서를 `Python 3.7+` 버전에 맞게 수정하였습니다.

<br><br>

## **기초 개념**

상속, 인터페이스, 오버라이딩

![image](https://github.com/user-attachments/assets/5690b685-0443-4588-a8f5-5c2cbb1a6014)

```python
from abc import ABC, abstractmethod

# 추상 클래스 Person
class Person(ABC):
    @abstractmethod
    def work(self):
        pass

# Firefighter 클래스, Person 클래스의 work 메서드를 오버라이딩
class Firefighter(Person):
    def work(self):
        print("The firefighter extinguishes fires.")

# Programmer 클래스, Person 클래스의 work 메서드를 오버라이딩
class Programmer(Person):
    def work(self):
        print("The programmer writes code.")

# PoliceOfficer 클래스, Person 클래스의 work 메서드를 오버라이딩
class PoliceOfficer(Person):
    def work(self):
        print("The police officer enforces the law.")

# 객체 생성 및 메서드 호출
firefighter = Firefighter()
programmer = Programmer()
police_officer = PoliceOfficer()

firefighter.work()    # The firefighter extinguishes fires.
programmer.work()     # The programmer writes code.
police_officer.work() # The police officer enforces the law.
```

<br>

오버로딩

```python
int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}
```

<br><br>

## **변수**

### 변수 이름은 의미가 있어야 하며, 발음할 수 있어야 합니다. 
(meaningful, pronounceable)


<br>

**나쁜 예:**

```python
import datetime

ymdstr = datetime.date.today().strftime("%y-%m-%d")
```

<br>

**좋은 예:**

```python
import datetime

current_date: str = datetime.date.today().strftime("%y-%m-%d")
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 동일 대상의 변수에 대해서는 동일 어휘를 사용합시다.

<br>

**나쁜 예:**
아래 예제는 동일 대상(entity)에 대해 3개의 다른 이름을 사용합니다.

```python
def get_user_info(): pass


def get_client_data(): pass


def get_customer_record(): pass
```

<br>

**좋은 예:**  
만약 entity가 동일하다면, 일관성 있게(consistent) 변수나 함수의 이름을 짓는 것이 좋습니다.

```python
def get_user_info(): pass


def get_user_data(): pass


def get_user_record(): pass
```

<br>

**참고하면 좋은 예:**  
Python은 객체 지향 프로그래밍 언어입니다. 필요한 경우 인스턴스의 속성(attribute), 프로퍼티 메소드(property method)나 메소드(method)와 함께 코드에서 entity의 구체적인 구현 및 패키지화하는 것이 좋습니다.

```python
from typing import Union, Dict


class Record:
    pass


class User:
    info: str

    @property
    def data(self) -> Dict[str, str]:
        return {}

    def get_record(self) -> Union[Record, None]:
        return Record()
```

**[⬆ 목차로 이동](#목차)**


<br><br>

### 검색에 용이한 이름을 사용합시다.
우리는 코딩을 하며 많은 코드를 읽습니다. 때문에 우리가 작성하는 코드를 읽기 쉽고 검색 가능한 이름으로 선언하는 것은 중요합니다. 

만약 변수를 선언할 때 의미가 없거나 검색에 어려움을 주는 이름으로 선언한다면, 우리의 코드를 읽는 다른 사람들이 힘들어할 것입니다.

검색 가능한(유추 가능한) 이름을 사용합시다.

<br>

**나쁜 예:**

```python
import time

# What is the number 86400 for again?
time.sleep(86400)
```

<br>

**좋은 예:**

```python
import time

# Declare them in the global namespace for the module.
SECONDS_IN_A_DAY = 60 * 60 * 24
time.sleep(SECONDS_IN_A_DAY)
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 읽는 사람으로 하여금 기능을 유추하도록 만드는 이름을 짓지 마세요.
변수가 의미하는 바가 무엇인지를 코드를 상세히 보지 않아도 알 수 있도록 하세요.

명시적인 것이 암묵적인 것보다 좋습니다.

<br>

**나쁜 예:**

```python
seq = ("Austin", "New York", "San Francisco")

for item in seq:
    # do_stuff()
    # do_some_other_stuff()

    # Wait, what's `item` again?
    print(item)
```

<br>

**좋은 예:**

```python
locations = ("Austin", "New York", "San Francisco")

for location in locations:
    # do_stuff()
    # do_some_other_stuff()
    # ...
    print(location)
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 불필요한 context는 추가하지 마세요.

클래스/객체 이름이 무언가를 이미 알려주는 경우, 변수 이름에서 이를 반복하지 마세요.

<br>

**나쁜 예:**

```python
class Car:
    car_make: str
    car_model: str
    car_color: str
```

<br>

**좋은 예**:

```python
class Car:
    make: str
    model: str
    color: str
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### short circuiting 또는 conditionals 대신 default parameter를 사용하세요.

여기서 `short circuiting`은 논리 연산(and, or)를 의미합니다.

<br>

**다음과 같은 상황에서**:

```python
import hashlib


def create_micro_brewery(name):
    name = "Hipster Brew Co." if name is None else name
    slug = hashlib.sha1(name.encode()).hexdigest()
    # etc.
```

<br>

만약 위와 같이 조건문을 사용하는 것 대신 매개변수만을 사용하더라도 함수의 동작에 아무런 영향이 없다는 것을 알 수 있습니다.

우리는 위 코드를 아래와 같이 수정하고 싶을 것입니다.

<br>

**좋은 예**:

```python
import hashlib


def create_micro_brewery(name: str = "Hipster Brew Co."):
    slug = hashlib.sha1(name.encode()).hexdigest()
    # etc.
```

**[⬆ 목차로 이동](#목차)**

<br><br>

## **함수**

### 함수는 작업의 단위입니다.

함수는 소프트웨어 엔지니어링에서 가장 중요한 rule 중 하나입니다.

함수들이 하나 이상의 작업을 수행한다면 관리, 테스트 및 추론에 어려움을 겪을 것입니다.

<br>

추가로 함수의 길이는 20줄 안으로 쓰는 것이 좋습니다. 아무리 길어져도 50줄 안으로..

<br><br>

**나쁜 예:**

```python
from typing import List


class Client:
    active: bool


def email(client: Client) -> None:
    pass


def email_clients(clients: List[Client]) -> None:
    """Filter active clients and send them an email.
    """
    for client in clients:
        if client.active:
            email(client)
```

<br><br>

**좋은 예**:

```python
from typing import List


class Client:
    active: bool


def email(client: Client) -> None:
    pass


def get_active_clients(clients: List[Client]) -> List[Client]:
    """Filter active clients.
    """
    return [client for client in clients if client.active]


def email_clients(clients: List[Client]) -> None:
    """Send an email to a given list of clients.
    """
    for client in get_active_clients(clients):
        email(client)
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수의 들여쓰기

다중 들여쓰기는 코드 가독성을 저해합니다. 보통 maximum 2단 들여쓰기까지 사용하는 것이 좋습니다.

<br><br>

**나쁜 예:**

```python
def process_data(data):
    for item in data:
        if item > 10:
            if item % 2 == 0:
                if item < 50:
                    print("Item is even, greater than 10, and less than 50")
                else:
                    print("Item is even, greater than 10, but 50 or more")
            else:
                print("Item is odd and greater than 10")
        else:
            print("Item is 10 or less")

```

<br><br>

**좋은 예**:

```python
def describe_large_item(item):
    """아이템이 10보다 크면 설명하는 함수"""
    if item % 2 != 0:
        print("Item is odd and greater than 10")
        return

    if item < 50:
        print("Item is even, greater than 10, and less than 50")
    else:
        print("Item is even, greater than 10, but 50 or more")

def process_data(data):
    """데이터 목록을 처리하고 아이템을 설명하는 함수"""
    for item in data:
        if item <= 10:
            print("Item is 10 or less")
            continue

        describe_large_item(item)
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수의 매개변수 (이상적으로 2개 이하)

매개변수의 수가 많다는 것은 일반적으로 함수가 너무 많은 일을 수행한다는 것을 의미합니다. (has more than one responsibility)

때문에 매개변수의 개수를 제한한다면 함수를 더 쉽게 테스트 할 수 있습니다. 

매개변수가 많은 함수를 매개변수가 적은 함수로 분해할 수 있다면 해보세요. 이상적으로는 3개 미만입니다.

<br>

함수에 대해 단일 책임(single responsibility)이 있는 경우 여러 개의 매개변수를 하나의 특수한 개체로 묶을 수 있는지도 살펴보세요.

프로그램에서 다른 곳에 매개변수를 재사용해야 하는 상황이 온다면 이 개체를 요긴하게 사용할 수 있습니다.

<br>

또한 이 방법이 여러 개의 매개변수를 갖는 것 보다 더 나은 이유는

함수 내부의 매개변수를 사용하여 수행되는 연산들을 또 하나의 함수로 만들어 복잡성을 줄일 수 있기 때문입니다.


<br>

**나쁜 예:**

```python
def create_menu(title, body, button_text, cancellable):
    pass
```

<br>

**좋은 예 1:**

```python
class MenuConfig:
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False


def create_menu(config: MenuConfig) -> None:
    title = config.title
    body = config.body
    # ...


config = MenuConfig()
config.title = "My delicious menu"
config.body = "A description of the various items on the menu"
config.button_text = "Order now!"
# The instance attribute overrides the default class attribute.
config.cancellable = True

create_menu(config)
```

<br>

**좋은 예 2:**

```python
from typing import NamedTuple


class MenuConfig(NamedTuple):
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False


def create_menu(config: MenuConfig):
    title, body, button_text, cancellable = config
    # ...


create_menu(
    MenuConfig(
        title="My delicious menu",
        body="A description of the various items on the menu",
        button_text="Order now!"
    )
)
```

<br>

**좋은 예 3 (Python3.8+ only)**

```python
from typing import TypedDict


class MenuConfig(TypedDict):
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool


def create_menu(config: MenuConfig):
    title = config["title"]
    # ...


create_menu(
    # You need to supply all the parameters
    MenuConfig(
        title="My delicious menu",
        body="A description of the various items on the menu",
        button_text="Order now!",
        cancellable=True
    )
)
```

**[⬆ 목차로 이동](#목차)**

<br><br>


### 함수의 이름은 함수가 수행하는 작업을 나타내야 합니다.

<br>

**나쁜 예:**

```python
class Email:
    def handle(self) -> None:
        pass


message = Email()
# What is this supposed to do again?
message.handle()
```

<br>

**좋은 예:**

```python
class Email:
    def send(self) -> None:
        """Send this message"""


message = Email()
message.send()
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수의 추상화(abstraction) 수준을 통일해라.

만약 함수에 추상적인 층이 하나 이상 있다면, 함수가 너무 복잡해집니다.

추상층이 여러 개 있다면, 그것들을 함수로 분해하여 재사용성을 높이고 테스트에 용이하도록 하는 것이 좋습니다.

<br>

***고수준 추상화는 복잡한 시스템의 전반적인 흐름과 개념을 다루며, 사용자에게 친숙한 인터페이스를 제공합니다.
저수준 추상화는 세부적인 구현과 알고리즘을 다루며, 성능 최적화와 같은 구체적인 요구사항에 대응합니다.***

**나쁜 예:**

```python
def process_order(order):
    # 고수준: 주문 처리
    print(f"Processing order for {order['customer']}")

    # 저수준: 세부 작업 - 총액 계산
    total = 0
    for item in order['items']:
        total += item['price'] * item['quantity']
    
    # 저수준: 세부 작업 - 세금 계산
    tax = total * 0.1
    
    # 저수준: 세부 작업 - 배송비 추가
    if total > 50:
        shipping = 0
    else:
        shipping = 5
    
    # 고수준: 결과 출력
    print(f"Total cost for the order is {total + tax + shipping}")
```

<br>

**좋은 예:**

```python
def process_order(order):
    # 고수준: 주문 처리
    print(f"Processing order for {order['customer']}")
    
    # 고수준: 계산된 총액을 사용해 처리
    total_cost = calculate_total_cost(order)
    
    # 고수준: 결과 출력
    print(f"Total cost for the order is {total_cost}")

def calculate_total_cost(order):
    # 저수준: 세부 작업 분리
    total = calculate_items_total(order)
    tax = calculate_tax(total)
    shipping = calculate_shipping(total)
    
    return total + tax + shipping

def calculate_items_total(order):
    total = 0
    for item in order['items']:
        total += item['price'] * item['quantity']
    return total

def calculate_tax(total):
    return total * 0.1

def calculate_shipping(total):
    return 0 if total > 50 else 5
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수의 매개변수로 flag를 사용하지 마세요.

flag는 사용자로 하여금 이 함수가 두가지 이상의 기능을 수행한다는 것으로 보여질 수 있습니다.

함수는 한가지 일을 해야합니다. bool을 기준으로 함수의 기능이 완전히 바뀐다면 함수를 분할해보세요.

<br>

**나쁜 예:**

```python
from tempfile import gettempdir
from pathlib import Path


def create_file(name: str, temp: bool) -> None:
    if temp:
        (Path(gettempdir()) / name).touch()
    else:
        Path(name).touch()
```

<br>

**좋은 예:**

```python
from tempfile import gettempdir
from pathlib import Path


def create_file(name: str) -> None:
    Path(name).touch()


def create_temp_file(name: str) -> None:
    (Path(gettempdir()) / name).touch()
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수는 부작용(side effect)을 피해야 합니다. 

여기서 말하는 부작용(side effect)은 부정적인 의미가 아닙니다. 

함수는 일반적으로 매개변수를 받은 후 일련의 작업을 거쳐 값을 반환합니다. 

만약 값을 반환하는 것 이외에 다른 작업을 추가로 수행하는 경우 이 행위를 부작용이라 부릅니다.

<br>

예를 들어 부작용으로 파일에 글을 쓸 수도 있으며, 파일의 특정 변수를 수정할 수도 있고, 실수로 모든 돈을 낯선 사람에게 송금할 수도 있습니다.

만약 부작용을 꼭 필요로 한다면, 부작용이 유발되는 위치를 표시해주는 것이 좋습니다. 

또한 다른 함수나 클래스가 동시에 동일한 파일을 조작하지 않도록 하고 특정 함수를 통해 파일을 이 파일을 조작하도록 합시다.

<br>

주요 요점은 개체 간 상태 공유, 가변 데이터 등을 사용하여 모든 함수 또는 변수가 이러한 데이터(파일 혹은 파일 내 데이터)를 조작할 수 있게 되는 일반적인 함정은 피할 필요가 있습니다. 

만약 이것을 잘 지킨다면, 다른 프로그래머들보다 오류를 찾기 더 수월해질 것입니다.

<br>

**나쁜 예:**

```python
# type: ignore

# This is a module-level name.
# It's good practice to define these as immutable values, such as a string.
# However...
fullname = "Ryan McDermott"


def split_into_first_and_last_name() -> None:
    # The use of the global keyword here is changing the meaning of the
    # the following line. This function is now mutating the module-level
    # state and introducing a side-effect!
    global fullname
    fullname = fullname.split()


split_into_first_and_last_name()

# MyPy will spot the problem, complaining about 'Incompatible types in
# assignment: (expression has type "List[str]", variable has type "str")'
print(fullname)  # ["Ryan", "McDermott"]

# OK. It worked the first time, but what will happen if we call the
# function again?
```

<br>

**좋은 예 1:**

```python
from typing import List, AnyStr


def split_into_first_and_last_name(name: AnyStr) -> List[AnyStr]:
    return name.split()


fullname = "Ryan McDermott"
name, surname = split_into_first_and_last_name(fullname)

print(name, surname)  # => Ryan McDermott
```

<br>

**좋은 예 2:**

```python
from dataclasses import dataclass


@dataclass
class Person:
    name: str

    @property
    def name_as_first_and_last(self) -> list:
        return self.name.split()


# The reason why we create instances of classes is to manage state!
person = Person("Ryan McDermott")
print(person.name)  # => "Ryan McDermott"
print(person.name_as_first_and_last)  # => ["Ryan", "McDermott"]
```

**[⬆ 목차로 이동](#목차)**

<br><br>

## **주석**

todo와 같은 주석 처럼 사용해도 되는 주석이 있지만,

주석은 되도록 쓰지 않고 코드를 보고 이해할 수 있도록 하는 것이 좋습니다.

코드는 변경되지만 주석은 변경되지 않으면서 주석이 코드를 따라가지 못 하는 경우도 있고, 

주석이 쌓이다보면 코드 가독성이 저해됩니다.

<br>

**나쁜 예:**

```python
//직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if(employee.flags & HOURLY_FLAG) && (employee.age > 65)
```

<br>

**좋은 예:**
```python
if(employee.isEligibleForFullBenefit())
```

**[⬆ 목차로 이동](#목차)**

<br><br>

## **형식맞추기**

코드 가독성을 위해 한 파일 당 200줄 안으로 작성, 길어져도 500줄 안으로 작성할 수 있도록 하는 것이 좋습니다.

비슷한 개념은 행끼리 묶고 아닌 것은 빈 공간으로 구분하는 것이 좋습니다.

한 라인은 스크린 화면에 스크롤 없이 한 번에 볼 수 있도록 하는 것이 좋으며, 

아무리 길어져도 80~120자 안으로 작성하는 것이 좋습니다.

<br>

**나쁜 예:**

```python
def process_order(order):
    total_price = 0
    discount = 0
    items = order['items']   # 물품 목록
    customer = order['customer']  # 고객 정보
    for item in items:total_price += item['price']*item['quantity'] #총 가격 계산
    if customer['loyalty_level'] > 3:discount = 0.1 * total_price  #VIP 고객 할인
    final_price = total_price - discount  #할인 적용
    print("Total price:", total_price)
    print("Discount:", discount)
    print("Final price:", final_price)
    return final_price
def send_confirmation_email(customer, final_price): print(f"Email sent to {customer['email']} with total price: {final_price}")

```

<br>

**좋은 예:**
```python
def process_order(order):
    # 주문 정보에서 물품 목록과 고객 정보 추출
    items = order['items']   
    customer = order['customer']  
    
    # 총 가격 계산
    total_price = 0
    for item in items:
        total_price += item['price'] * item['quantity']
    
    # VIP 고객일 경우 할인 적용
    discount = 0
    if customer['loyalty_level'] > 3:
        discount = 0.1 * total_price  # 10% 할인

    # 최종 가격 계산
    final_price = total_price - discount  
    
    # 결과 출력
    print("Total price:", total_price)
    print("Discount:", discount)
    print("Final price:", final_price)

    return final_price

def send_confirmation_email(customer, final_price):
    """이메일 확인 메시지 발송"""
    print(f"Email sent to {customer['email']} with total price: {final_price}")

```

**[⬆ 목차로 이동](#목차)**

<br><br>

## **클래스**

### 단일 책임 원칙 (Single Responsibility Principle; SRP)

*설명하기에 앞서 책임(responsibility)를 이해를 위해 기능으로 해석했음을 미리 알려드리겠습니다.*

<br>

Robert C. Martin이 말하기를...:

> Class가 변경될 이유는 단 하나여야 한다. (A class should have only one reason to change.)

"변경되야 할 이유"는 클래스 또는 함수가 담당하는 기능에 대응합니다.

다음 예제에서는 HTML 주석을 만들고 주석에 pip의 버전을 기록합니다:

<br>

**나쁜 예:**

```python
from importlib import metadata


class VersionCommentElement:
     """An element that renders an HTML comment with the program's version number
     """

     def get_version(self) -> str:
          """Get the package version"""
          return metadata.version("pip")

     def render(self) -> None:
          print(f'<!-- Version: {self.get_version()} -->')


VersionCommentElement().render()
```

<br>

위 클래스는 두가지 기능이 있습니다.
- pip 버전 정보를 획득합니다.
- HTML 주석을 생성합니다.

다만 위 코드에서 특정 기능을 변경하면 다른 기능에 영향을 미칩니다.  
우리는 이 두 기능을 분해할 수 있습니다.

<br>

**좋은 예:**

```python
from importlib import metadata


def get_version(pkg_name: str) -> str:
     """Retrieve the version of a given package"""
     return metadata.version(pkg_name)


class VersionCommentElement:
     """An element that renders an HTML comment with the program's version number
     """

     def __init__(self, version: str):
          self.version = version

     def render(self) -> None:
          print(f'<!-- Version: {self.version} -->')


VersionCommentElement(get_version("pip")).render()
```

<br>

위와 같이 작성하면 이 클래스는 **HTML 요소를 생성하는 것에만 집중**하게 됩니다.

인스턴스화할 때 버전 번호가 초기 매개변수로 전달됩니다. (`get_version()`을 통해 버전 정보를 얻음)

클래스 및 함수는 서로 격리되어 있으며 버전 사항이 다른 항목에는 영향을 미치지 않습니다.

또한 `get_version()`은 재사용될 수 있습니다.

**[⬆ 목차로 이동](#목차)**

<br><br>

### 개방/폐쇄 원칙 (Open/Closed Principle; OCP)

소프트웨어의 객체(클래스, 함수 등)는 확장(extension)에 대해 열려 있어야 하지만, 수정(modification)에는 닫혀있어야 합니다.

클래스 같은 개체는 내부 논리를 수정하지 않고 새로운 기능을 추가할 수 있도록 보장해야합니다.   
(원래 코드를 수정하지 않으면서 코드를 추가할 수 있어야 한다는 의미와 같습니다.)

즉, 객체는 설계 초기에 확장성을 보장해야 합니다.

다음 예에서는 HTTP 요청에 응답하는 간단한 웹 프레임워크를 구현하는 코드입니다.

HTTP 서버에서 GET 요청을 받으면 `View` 클래스 `.get()` 메소드가 호출됩니다.

<br>

`View`는 단순히 `text/plain`만 반환합니다.

하지만 우리는 `text/HTML`의 형태로 받기를 원합니다.

그래서 우리는 `View` 클래스를 상속받아 `TemplateView` 클래스를 만들었습니다.

<br>

**나쁜 예:**

```python
from dataclasses import dataclass


@dataclass
class Response:
     """An HTTP response"""

     status: int
     content_type: str
     body: str


class View:
     """A simple view that returns plain text responses"""

     def get(self, request) -> Response:
          """Handle a GET request and return a message in the response"""
          return Response(
               status=200,
               content_type='text/plain',
               body="Welcome to my web site"
          )


class TemplateView(View):
     """A view that returns HTML responses based on a template file."""

     def get(self, request) -> Response:
          """Handle a GET request and return an HTML document in the response"""
          with open("index.html") as fd:
               return Response(
                    status=200,
                    content_type='text/html',
                    body=fd.read()
               )

```

<br>

새로운 기능을 구현하기 위해 `TemplateView`는 `View`를 상속받고 `.get()` 메소드를 다시 썼습니다.

위 코드는 부모 클래스의 `.get()`을 변경하지 않고 자식 클래스에서 오버라이딩 한 경우입니다.

<br>

만약 위와 같은 방식으로 기능이 여러 개로 파생된다면,   
테스트를 수행할 때 `View` 클래스의 모든 자식 클래스에 대해 테스트 기능을 추가해야할 가능성이 있습니다.

이 문제를 해결하기 위해 코드를 다시 설계하고 `View` 클래스가 깨끗하게 확장되도록 합시다.

<br>

**좋은 예:**

```python
from dataclasses import dataclass


@dataclass
class Response:
     """An HTTP response"""

     status: int
     content_type: str
     body: str


class View:
     """A simple view that returns plain text responses"""

     content_type = "text/plain"

     def render_body(self) -> str:
          """Render the message body of the response"""
          return "Welcome to my web site"

     def get(self, request) -> Response:
          """Handle a GET request and return a message in the response"""
          return Response(
               status=200,
               content_type=self.content_type,
               body=self.render_body()
          )


class TemplateView(View):
     """A view that returns HTML responses based on a template file."""

     content_type = "text/html"
     template_file = "index.html"

     def render_body(self) -> str:
          """Render the message body as HTML"""
          with open(self.template_file) as fd:
               return fd.read()

```

<br>

응답 내용을 변경하려면 `render_body()`를 재정의해야 하지만  
이 메소드는 하위 유형을 재정의하도록 요청하는 잘 정의된 단일 책임(single reponsibility)이 있습니다.

<br>

**[⬆ 목차로 이동](#목차)**

<br><br>

### 리스코프 치환 원칙 (Liskov Substitution Principle; LSP)

<br>

> "부모 클래스의 포인터나 참조를 사용하는 함수는  
> 부모 클래스로부터 파생된 자식 클래스에 대해 몰라도 사용할 수 있어야 해.", Uncle Bob.

<br>

이 원칙은 *A behavioral notion of subtyping (1994)* 논문의 저자 Jeannette Wing과 협력한 Barbara Liskov의 이름을 따서 명명되었습니다.

이 논문의 핵심 원칙은 "subtype이 supertype와 동일한 방법과 동일한 기능과 동일 행동을 유지해야 한다"는 것입니다.

다시 말해 supertype의 함수는 별도의 수정 없이 모든 subtype을 수용할 수 있어야 합니다.

<br>

아래의 코드에서 어떤 문제가 있는지 확인해보도록 합시다.

**나쁜 예:**

```python
from dataclasses import dataclass


@dataclass
class Response:
     """An HTTP response"""

     status: int
     content_type: str
     body: str


class View:
     """A simple view that returns plain text responses"""

     content_type = "text/plain"

     def render_body(self) -> str:
          """Render the message body of the response"""
          return "Welcome to my web site"

     def get(self, request) -> Response:
          """Handle a GET request and return a message in the response"""
          return Response(
               status=200,
               content_type=self.content_type,
               body=self.render_body()
          )


class TemplateView(View):
     """A view that returns HTML responses based on a template file."""

     content_type = "text/html"

     def get(self, request, template_file: str) -> Response:  # type: ignore
          """Render the message body as HTML"""
          with open(template_file) as fd:
               return Response(
                    status=200,
                    content_type=self.content_type,
                    body=fd.read()
               )


def render(view: View, request) -> Response:
     """Render a View"""
     return view.get(request)
    
render(TemplateView(), None)

```

<br>

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-20-e8bfee3ef0dd> in <cell line: 50>()
     48      return view.get(request)
     49 
---> 50 render(TemplateView(), None)

<ipython-input-20-e8bfee3ef0dd> in render(view, request)
     46 def render(view: View, request) -> Response:
     47      """Render a View"""
---> 48      return view.get(request)
     49 
     50 render(TemplateView(), None)

TypeError: TemplateView.get() missing 1 required positional argument: 'template_file'

<br>

`render()` 메소드는 `View` 클래스 및 하위 클래스인 `TemplateView`와 함께 사용할 수 있어야 합니다.

하지만 `TemplateView`는 상속 시 `.get()` 메소드의 signature(메소드의 입/출력)을 변경했습니다.

`TemplateView`의 `render()`를 사용할 경우 오류가 발생할 것입니다.

<br>

**좋은 예:**

```python
from dataclasses import dataclass
from typing import Optional


@dataclass
class Response:
    """An HTTP response"""

    status: int
    content_type: str
    body: str


class View:
    """A simple view that returns plain text responses"""

    content_type = "text/plain"

    def render_body(self) -> str:
        """Render the message body of the response"""
        return "Welcome to my web site"

    def get(self, request) -> Response:
        """Handle a GET request and return a message in the response"""
        return Response(
            status=200,
            content_type=self.content_type,
            body=self.render_body()
        )


class TemplateView(View):
    """A view that returns HTML responses based on a template file."""

    content_type = "text/html"
    template_file: Optional[str] = None  # 추가: template_file 속성

    def __init__(self, template_file: str):
        self.template_file = template_file

    def get(self, request) -> Response:  # 타입 수정
        """Render the message body as HTML"""
        if not self.template_file:
            raise ValueError("Template file must be provided")

        with open(self.template_file) as fd:
            return Response(
                status=200,
                content_type=self.content_type,
                body=fd.read()
            )


def render(view: View, request) -> Response:
    """Render a View"""
    return view.get(request)

render(TemplateView('index.html'), None)
```

<br>

출력 결과 : Response(status=200, content_type='text/html', body='test html')

<br>

**[⬆ 목차로 이동](#목차)**

<br><br>

### 인터페이스 분리 원칙 (Interface Segregation Principle; ISP)

<br>

> “사용자가 필요없는 것에 의존하지 않도록 인터페이스를 간결하게 만드는 건 어때?", Uncle Bob.

<br>

Java나 Go와 같은 유명한 객체 지향 프로그래밍 언어에서는 인터페이스(interface)라는 개념이 있습니다.

인터페이스 클래스는 공개 메소드와 속성을 구현하지 않고 정의합니다.

함수의 signature(함수의 입/출력)를 정의하고 싶지만 구체적으로 구현하고 싶지 않을 때 인터페이스는 매우 유용하게 사용됩니다.

<br>

우리는 "당신이 나에게 전달한 대상의 세부 사항에 대해서는 관심이 없고, 내가 사용할 수 있는 방법이나 속성에만 관심이 있다."고 말할 수 있습니다.

<br>

*Python에는 인터페이스가 없습니다.*

다만 인터페이스와는 약간 다르지만 추상 클래스를 사용하여 동일한 기능을 구현할 수 있습니다.

<br>

**좋은 예:**

```python

from abc import ABCMeta, abstractmethod


# Define the Abstract Class for a generic Greeter object
class Greeter(metaclass=ABCMeta):
     """An object that can perform a greeting action."""

     @staticmethod
     @abstractmethod
     def greet(name: str) -> None:
          """Display a greeting for the user with the given name"""


class FriendlyActor(Greeter):
     """An actor that greets the user with a friendly salutation"""

     @staticmethod
     def greet(name: str) -> None:
          """Greet a person by name"""
          print(f"Hello {name}!")


def welcome_user(user_name: str, actor: Greeter):
     """Welcome a user with a given name using the provided actor"""
     actor.greet(user_name)


welcome_user("Barbara", FriendlyActor())
```

<br>

이제 다음의 시나리오를 상상해봅시다. 

PDF 문서가 몇 개 있는데, 우리 웹 사이트 사용자에게 PDF 파일을 제공하고 싶습니다.

우리는 파이썬 웹 프레임워크를 사용하여 이러한 문서를 관리하기 위한 클래스를 설계하고자 합니다.

그래서 우리는 문서에 추상 클래스를 설계했는데, 이 클래스에 사용할 수 있는 모든 기능들을 적어두었습니다.

<br>

**에러 발생:**

```python
import abc


class Persistable(metaclass=abc.ABCMeta):
     """Serialize a file to data and back"""

     @property
     @abc.abstractmethod
     def data(self) -> bytes:
          """The raw data of the file"""

     @classmethod
     @abc.abstractmethod
     def load(cls, name: str):
          """Load the file from disk"""

     @abc.abstractmethod
     def save(self) -> None:
          """Save the file to disk"""


# We just want to serve the documents, so our concrete PDF document
# implementation just needs to implement the `.load()` method and have
# a public attribute named `data`.

class PDFDocument(Persistable):
     """A PDF document"""

     @property
     def data(self) -> bytes:
          """The raw bytes of the PDF document"""
          ...  # Code goes here - omitted for brevity

     @classmethod
     def load(cls, name: str):
          """Load the file from the local filesystem"""
          ...  # Code goes here - omitted for brevity


def view(request):
     """A web view that handles a GET request for a document"""
     requested_name = request.qs['name']  # We want to validate this!
     return PDFDocument.load(requested_name).data

```

<br>

하지만 안되더라고요! `.save()` 메소드를 구현하지 않으면 예외가 발생합니다.

```
Can't instantiate abstract class PDFDocument with abstract method save.
```

이건 짜증나네요. 우리는 `.save()`를 구현할 필요가 없습니다.

우리는 아무것도 하지 않거나 `NotImplementedError`를 발생시키는 더미 메소드를 구현할 수 있지만, 그것또한 쓸모없는 코드가 됩니다.

<br>

동시에 만약 우리가 추상 클래스에서 `.save()`를 제거한다면, 사용자가 문서를 save 할 때 다시 추가해야 합니다.

<br>

문제를 요약하면, 우리는 인터페이스를 썼고, 이 인터페이스에는 현재 사용할 수 없는 몇가지 특성이 있다는 것입니다.

이 문제에 대한 해결방식은 이 인터페이스를 더 작은 인터페이스로 분해하고, 각각의 새로운 인터페이스가 일부 내용을 담당하도록 만드는 것입니다.

<br>

**좋은 예:**

```python
import abc


class DataCarrier(metaclass=abc.ABCMeta):
     """Carries a data payload"""

     @property
     def data(self):
          ...


class Loadable(DataCarrier):
     """Can load data from storage by name"""

     @classmethod
     @abc.abstractmethod
     def load(cls, name: str):
          ...


class Saveable(DataCarrier):
     """Can save data to storage"""

     @abc.abstractmethod
     def save(self) -> None:
          ...


class PDFDocument(Loadable):
     """A PDF document"""

     @property
     def data(self) -> bytes:
          """The raw bytes of the PDF document"""
          ...  # Code goes here - omitted for brevity

     @classmethod
     def load(cls, name: str) -> None:
          """Load the file from the local filesystem"""
          ...  # Code goes here - omitted for brevity


def view(request):
     """A web view that handles a GET request for a document"""
     requested_name = request.qs['name']  # We want to validate this!
     return PDFDocument.load(requested_name).data

```

<br>

**[⬆ 목차로 이동](#목차)**

<br><br>

### 의존성 역전 원칙 (Dependency Inversion Principle; DIP)

<br>

> "구체적인 세부 사항(details)보다는 추상(abstractions)에 의존하는 건 어때?", Uncle Bob.

> DIP의 핵심 아이디어

고수준 모듈은 저수준 모듈의 세부 구현에 의존하지 않고, 추상화된 인터페이스에 의존해야 합니다.

저수준 모듈 역시 직접적으로 고수준 모듈에 의존하지 않고, 추상화된 인터페이스를 통해 상호작용해야 합니다.

의존성의 방향은 구체적인 구현이 아닌, 추상화된 계층으로 향하게 만들어야 합니다.

<br>

아래 코드는 DIP를 위반한 예입니다. OrderService (고수준 모듈)가 EmailService (저수준 모듈)의 구체적인 구현에 직접 의존하고 있습니다.

고수준 모듈은 일반적으로 다양한 저수준 모듈을 사용하여 목적을 달성합니다.

저수준 모듈은 시스템의 구체적인 세부 사항이나 기술적 구현을 담당합니다.

<br>

**Bad**

```python
class EmailService:
    def send_email(self, recipient: str, message: str) -> None:
        print(f"Sending email to {recipient}: {message}")

class OrderService:
    def __init__(self, email_service: EmailService):
        self.email_service = email_service

    def place_order(self, order: str, recipient: str) -> None:
        print(f"Placing order: {order}")
        self.email_service.send_email(recipient, "Your order has been placed.")
```

<br>

OrderService가 EmailService의 구체적인 구현에 의존하고 있습니다. 

만약 이메일 이외의 방식(예: SMS, 푸시 알림 등)으로 알림을 보내고 싶다면 OrderService의 코드를 수정해야 합니다. 

이로 인해 유연성과 확장성이 떨어집니다.

<br>

DIP를 준수하려면, OrderService와 EmailService 사이에 추상화 계층을 두어 
OrderService가 구체적인 EmailService 구현에 의존하지 않도록 해야 합니다.

<br>

**Good**

```python
from abc import ABC, abstractmethod

# 추상화된 인터페이스 정의
class NotificationService(ABC):
    @abstractmethod
    def send(self, recipient: str, message: str) -> None:
        pass

# 저수준 모듈: EmailService는 NotificationService 인터페이스를 구현
class EmailService(NotificationService):
    def send(self, recipient: str, message: str) -> None:
        print(f"Sending email to {recipient}: {message}")

# 저수준 모듈: SMSService도 NotificationService 인터페이스를 구현
class SMSService(NotificationService):
    def send(self, recipient: str, message: str) -> None:
        print(f"Sending SMS to {recipient}: {message}")

# 고수준 모듈: OrderService는 NotificationService 인터페이스에 의존
class OrderService:
    def __init__(self, notification_service: NotificationService):
        self.notification_service = notification_service

    def place_order(self, order: str, recipient: str) -> None:
        print(f"Placing order: {order}")
        self.notification_service.send(recipient, "Your order has been placed.")

# 사용 예
email_service = EmailService()
order_service = OrderService(email_service)
order_service.place_order("Laptop", "customer@example.com")

# SMS 서비스로 변경
sms_service = SMSService()
order_service_sms = OrderService(sms_service)
order_service_sms.place_order("Phone", "123-456-7890")

```

<br>

**추상화된 인터페이스 NotificationService**를 도입하여, 고수준 모듈인 OrderService가 구체적인 EmailService나 SMSService에 의존하지 않도록 했습니다.

OrderService는 이제 추상화된 인터페이스(NotificationService)에 의존하므로, 알림 방식(이메일, SMS, 푸시 알림 등)을 변경하려면 단지 인터페이스를 구현한 다른 클래스(SMSService 등)를 주입하기만 하면 됩니다.

이로 인해 유연성이 높아지고, 새로운 알림 방식이 추가되더라도 고수준 모듈인 OrderService는 수정할 필요가 없습니다.

<br>

**[⬆ 목차로 이동](#목차)**

<br><br>

## **반복은 지양합시다. (Don't repeat yourself; DRY)**

위키피디아의 [중복 배제 원칙](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) 문서를 살펴보고 오세요.

<br>

중복 코드는 코드 로직을 수정할 때 중복되는 부분도 동시에 수정해야 한다는 것을 의미합니다.

중복 코드가 많으면 많을 수록 수정 작업량이 많아질 수 밖에 없고 오류 발생 가능성 또한 높아지게 됩니다.

<br>

식당을 운영하고 토마토, 양파, 마늘, 향신료 등 재고를 조사한다고 생각해봅시다.

리스트가 여러 개 있으면 토마토 하나로 토마토가 들어간 요리를 만들었을 때 여러 개의 리스트를 전부 업데이트 해야 합니다.

반대로 목록이 하나만 있으면 하나만 업데이트 하면 됩니다.

<br>

공통점이 많지만 코드 상에 약간 다른 것이 있어서 중복 코드를 사용하는 경우가 종종 있습니다.

하지만 그 차이로 인해 동일한 작업을 수행하는 두 개 이상의 개별 함수가 필요합니다.

중복 코드를 제거하려면 먼저 공통 부분을 추상화한 다음 하나의 함수/모듈/클래스로 다른 부분을 처리해야 합니다.

<br>

추상적 사고를 잘 하는 것은 프로그래머에게 있어 매우 중요한 스킬 중 하나입니다.

나쁜 추상적 사고로 인한 피해는 때때로 중복 코드보다 더 심각한 문제에 직면할 수 있습니다.

만약 추상적 사고를 잘 할 수 있다면, 그렇게 하셔야 합니다! 중복 코드를 작성하지 맙시다.

이를 지키지 않는다면 로직을 변경하고자 할 때 변경해야할 부분이 많다는 것을 곧 알게 될 것입니다.

<br>

**추상적 사고를 잘 한다는 것:**  
번역을 하며 제일 이해하기 어려웠던 부분이 바로 abstraction이라는 말인데,  
맥락을 보면 공통적이고 본질적인 부분만 추출하고 개별적인 부분은 버린다는 의미 같습니다. 

<br>

**나쁜 예:**

```python
from typing import List, Dict
from dataclasses import dataclass


@dataclass
class Developer:
    def __init__(self, experience: float, github_link: str) -> None:
        self._experience = experience
        self._github_link = github_link

    @property
    def experience(self) -> float:
        return self._experience

    @property
    def github_link(self) -> str:
        return self._github_link


@dataclass
class Manager:
    def __init__(self, experience: float, github_link: str) -> None:
        self._experience = experience
        self._github_link = github_link

    @property
    def experience(self) -> float:
        return self._experience

    @property
    def github_link(self) -> str:
        return self._github_link


def get_developer_list(developers: List[Developer]) -> List[Dict]:
    developers_list = []
    for developer in developers:
        developers_list.append({
            'experience': developer.experience,
            'github_link': developer.github_link
        })
    return developers_list


def get_manager_list(managers: List[Manager]) -> List[Dict]:
    managers_list = []
    for manager in managers:
        managers_list.append({
            'experience': manager.experience,
            'github_link': manager.github_link
        })
    return managers_list


## create list objects of developers
company_developers = [
    Developer(experience=2.5, github_link='https://github.com/1'),
    Developer(experience=1.5, github_link='https://github.com/2')
]
company_developers_list = get_developer_list(developers=company_developers)

## create list objects of managers
company_managers = [
    Manager(experience=4.5, github_link='https://github.com/3'),
    Manager(experience=5.7, github_link='https://github.com/4')
]
company_managers_list = get_manager_list(managers=company_managers)
```

<br>

**Good:**

```python
from typing import List, Dict
from dataclasses import dataclass


@dataclass
class Employee:
    def __init__(self, experience: float, github_link: str) -> None:
        self._experience = experience
        self._github_link = github_link

    @property
    def experience(self) -> float:
        return self._experience

    @property
    def github_link(self) -> str:
        return self._github_link


def get_employee_list(employees: List[Employee]) -> List[Dict]:
    employees_list = []
    for employee in employees:
        employees_list.append({
            'experience': employee.experience,
            'github_link': employee.github_link
        })
    return employees_list


## create list objects of developers
company_developers = [
    Employee(experience=2.5, github_link='https://github.com/1'),
    Employee(experience=1.5, github_link='https://github.com/2')
]
company_developers_list = get_employee_list(employees=company_developers)

## create list objects of managers
company_managers = [
    Employee(experience=4.5, github_link='https://github.com/3'),
    Employee(experience=5.7, github_link='https://github.com/4')
]
company_managers_list = get_employee_list(employees=company_managers)
```

<br>

**[⬆ 목차로 이동](#목차)**

<br><br>

## **Translations**

이 문서는 다양한 언어로 번역되었습니다:

- 🇨🇳 **
  Chinese** [yinruiqing/clean-code-python](https://github.com/yinruiqing/clean-code-python)
- 🇰🇷 ** Korean ** [wooy0ng/clean-code-python](https://github.com/wooy0ng/clean-code-python)
- 🇵🇹 🇧🇷 **
  Portugese** [fredsonchaves07/clean-code-python](https://github.com/fredsonchaves07/clean-code-python)
- 🇮🇷 **
  Persian:** [SepehrRasouli/clean-code-python](https://github.com/SepehrRasouli/clean-code-python)

<br>

**[⬆ 목차로 이동](#목차)**

<br><br>
