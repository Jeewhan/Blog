---
layout: post
title: " 파이썬 기초"
subtitle: "파이썬 기초"
categories: Python
tags: Python
---

### 파이썬 기초

---

1일차

- 자료형
- 제어문
- 입력과 출력
- 함수
- PEP8



##### 자료형과 자료구조는 무엇이 다른가?

```
자료형 = Data Type / 프리미티브 타입(파이썬에서 이미 구현이 완료되어있는 애들) / 숫자, 문자, 리스트, 튜플, 딕셔너리, 셋 등
자료구조 = Data Structure / 데이터를 저장하는 방식 / 배열, 집합, 키-값 등
자료구조중 일부들이 자료형으로 이미 구현되어있음
배열 = 리스트, 집합 = 셋, 키-값 = 딕셔너리
그 언어는 어떤 자료형을 제공하고 있는지를 이해하는 것이 매우 중요합니다
또한 컴퓨터 분야 전반에서는 어떤 자료구조를 쓰고 있는가 에 대한 파악하는 것이 매우 중요하다
```
##### 자료형
```
Number : 정수, 실수, 복소수, 8진수, 16진수 등
String : 문자 또는 문자열
List : 순서가 있는 여러 요소들의 모음
Tuple : 리스트와 동일하지만, 불변
Dict : 키와 값이 연결되어있는 구조
Set : 순서가 없는 유니크 element들의 모음
Boolean : 참 또는 거짓
```
##### 파이썬 연산
```python
a += 1   # a에 1을 더해서 a에 할당
a ** 2   # a^2
14 // 3  # 몫
14 % 3   # 나머지
```
- 자료형
```python
# 변수명은 소문자
# room_size = Under Line / roomSize = CamelCase
# 파이썬에서는 변수명으로 무조건 Under Line 사용

# type(변수명) : 변수의 타입을 알려준다
# int(변수명) : 변수의 타입을 정수형태로 변경 / list(), set()

animals = ["강아지", "고양이", "이구아나", "물고기", "참새"]
# 하나씩 출력을 하다가, index가 2번인 경우 파이썬으로 변경하자
# name[0] : name 변수에서 0번 째만 추출
# name[1:] : 첫 번째부터 맨 끝까지 추출

# 문자열 중간에 변수의 값을 넣는 방법
# https://pyformat.info/

"%s님 안녕하세요." % ("안수찬")
"{0}님 안녕하세요.".format(‘안수찬’)
"{name}님 안녕하세요.".format(name="안수찬")
"안녕하세요. {name} 입니다. 저는 {course} 를 수강하고 있습니다.".format(name=안수찬, course="데사스")

# 주석처리

# List : 순서가 있는 요소들, 변경 가능
animals = ["강아지", "고양이", "이구아나"]
animals.append("물고기")
animals[0][0] # 강아지

# Tuple : List와 동일하지만, Element 값이 변경이 될 수 없다
width_and_height = (120, 240)
width, height = width_and_height
width = 120
height = 240

# Set : 집합, 순서가 없고, Unique한 값만 담길 수 있다, 중복 제거용
name_set = set(["안수찬", "안수찬", "김승현", "박승권"])
name_set == {‘김승현’, ‘박승권’, ‘안수찬’}

name_list = ["안수찬", "안수찬", "안수찬", "김승현", "김승현", "김승현", "박준영", "박준영", "박준영"]
name_list = list(set(name_list)) # 파이썬다운 중복을 제거하는 방법이되, 순서를 보장하지는 않음
name_list == [‘김승현’, ‘안수찬’, ‘박준영’]

# Boolean
True or False
# 특정 조건에서 작동하도록 설정할 때, 매우 많이 사용될 것
# & 나 | 보다는 and 또는 or로 쓰는 것이 보다 명시적이다
# and 와 or가 섞여있어야 한다면, 좀 더 직관적인 프로세스를 만들어보거나 ( )로 전후를 구분해줄 것

# Dict : 키와 값을 쌍으로 묶은 것, 다른 곳에서는 Dict, Hash, JSON 등으로 쓰임
# :은 키에 붙여야 한다
# 함수 안에 키-밸류 형태로 넣을 때는 띄어쓰기를 하지 않는다
detail_dict = {"name": "안수찬", "age": 24, "phone number": "010-1234-5678"}
detail_dict["age"] == ‘안수찬'

my_informations = {"name": "dobestan", "phonenumber": "010-1234-5678", "email": "test@gmail.com"}
```
---
##### 반복문과 제어문
```python
if 1 < 3:
     print("1이 3보다 작다.")

a = 13
b = 12

if a < b:
     print("a({a})가 b({b})보다 작다.").format(a=a, b=b))
else:
     print("a가 b보다 크다.").format(a=a, b=b))
     
animals = ["강아지", "고양이", "이구아나"]
if "이구아나" in animals:

     print("이구아나를 키우고 있습니다.")
else:
     print("이구아나를 키우고 있지 않습니다.")

# 90점 이상은 A
# 60 ~ 90점까지는 B
# 60점 밑으로는 C
score = 89

if score >= 90:
     print("A")
elif score >= 60:
     print("B")
else:
     print("C")

# 제어문 : 특정 조건 또는 반복적으로 코드를 실행
# for : while문보다 통제가 수월하고 직관적이기에 더 선호됨
# for문을 사용할 때 많이 사용할, range라는 내장함수를 제공한다
list(range(10)) # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

animals = ["강아지", "고양이", "참새", "이구아나"]
for animal in animals:
     print("나는 {animal}를 키운다.".format(animal=animal))
for number in range(10):
     print("hello world {number}".format(number=number))
for _ in range(10): # 변수를 for문 내에서는 사용하지 않을 때 _ 를 써줌
     print("hello world")

# while : Python에서는 for문을 더 선호함, 덜 직관적이기 때문, 지속적 통제가 요구됨
age = 20
while age < 30:
    print("20대에 나이를 먹었습니다. 현재 나이 : {age}".format(age=age))
    age += 1
```
```python
# *
# **
# ***
# ****
# *****

for i in range(5):
     star = ""
     for j in range(i+1):
          star += "*"
     print(star)

star = ""
for i in range(5):
     star += "*"
     print(star)

for i in range(5):
     print("*" * (i + 1))
```
```
별찍기 과제

*
**
***
****
*****

    *
   **
  ***
 ****
*****

*****
****
***
**
*

*****
 ****
  ***
   **
    *

# 별찍기 과제 해답

for i in range(5):
    print("*" * (i + 1))

for i in range(5):
    print(" "* (4 - i) + "*" * (i + 1))

for i in range(5):
    print("*" * (5 - i))

for i in range(5):
    print(" " * i + "*" * (5 - i))
```
##### 입력과 출력

- 사용자 / 파일 / stdin ( standard input ) / stdout ( standard output )

##### 사용자로부터 인풋 받기

```python
username = input("너의 이름은 뭐니? ")

for i in range(int(input("별 몇개? "))):
     print("*" * (i + 1))
```
##### Python 3 VS Python 2
```
Python 3.X => input (…), print(…)
input은 항상 str으로 정보를 받아온다
Python 2.X => input을 받은 정보를 파이썬으로 실행하려고 함
TEXT를 받고자 하면 raw_input 을 사용해야 함

Python 3.X => print 는 함수 / print(" ")
Python 2.X => print 는 statement, 출력하는 기능 / print " "

Unicode is a character set. ( 표준 문자열 집합 )
UTF-8 is an encoding. ( 유니코드를 표현할 수 있는 인코딩 방식 )
Unicode를 표현하는 방식 중 하나가 utf-8
Unicode를 encoding하는 방식이 utf-8
utf-8을 decode하면 Unicode
한글은 일반적으로 UTF-8 or EUC-KR ( 거의 대부분은 UTF-8 사용 )

Python 2.X => 기본 인코딩이 ascii여서 한글 작성시에 문제가 발생
그래서 아래와 같이 붙여놔야 함
-*- coding: utf-8 -*-
( 위 문장이 추가되면 읽을 때 str이 utf-8로 인코딩되었다고 인지한다 )

하지만 위 문장이 있다고 해서 decode, encode하는 과정에서의 default setting이 ascii가 아니게 되는 것은 아니므로 아무런 옵션 지정없이 decode, encode를 하게 되면 ascii 방식으로 인코딩되어서 에러가 발생하게 된다

( 파이썬에서는 문자열을 표현할 수 있는 str, unicode 타입을 제공한다 )
print(type('한글')) => str
print(type(u'한글')) => unicode

에러 예시: print str(unicode("한글"))
참 예시: print "한글".decode("utf8") => unicode
참 예시: print "한글".decode("utf8").encode("utf8") => str

unicode로 인코딩 되어있으면 str으로 저장됨
unicode로 인코딩 되어있지 않으면 unicode로 저장됨
(하지만 unicode를 print 할 때는 자동적으로 utf-8로 변환함)

다른 사람들이 작성한 파일을 쓸 때는, Python 기본 인코딩을 ascii에서 utf8로 변경해주면 된다
import sys
reload(sys)
sys.setdefaultencoding('utf-8')

Python 2.X 에서 한글 문제 없이 사용하기
1. 파일 상단에 -*- coding: utf-8 -*- 명시
2. 파이썬 스크립트 내에서는 반드시 unicode 로 변경해서 사용
3. 외부 파일을 읽거나 쓸 때는 str 로 변경해서 읽거나 쓴다
4. 외부 라이브러리를 쓸 떄는 setdefaultencoding 을 사용해서 기본적으로 utf-8 사용

Python 3.X 는 모든 문자열이 unicode 이기에 인코딩에 대해 고민할 필요없음
```
```python
# Indent로 포함관계 구분
for i in range(len(animals)):
    if i == 2:
        animals[2] = "파이썬"
    print(animals[i])

for animal in animals:
    index = animals.index(animal)
    if index == 2:
        animals[index] = "파이썬"
    print(animals[index])

for index, _ in enumerate(animals):
    if index == 2:
        animals[index] = "파이썬"
    print(animals[index])

for my_information in my_informations:
    print(my_information)
    print(my_informations[my_information])

for my_information in my_informations.items():
    print(my_information) # Tuple
    key, value = my_information
    print(key)
    print(value)

for key, value in my_informations.items():
    print(key)
    print(value)

my_informations.keys()
```
---
##### 파일 입출력
```python
# ./ => 현재 폴더
# ../ => 상위 폴더
# ./test.txt => 현재 폴더에 있는 test.txt
# ../test.txt => 상위 폴더에 있는 test.txt
# ../../test.txt => 상위 폴더의 상위 폴더에 있는 test.txt

f = open("../animals.txt", "r") # mode => "w", "r", "a" ( append )
# f.read() => 파일의 전체 데이터
# f.readline() => 실행할 때마다 행이 바뀌어 출력
# f.readlines() => 각각의 라인별로 구분되어 리스트

f = open("./animals.txt", "w")
f.write("hello, world")
f.close

# 위와 동일하지만 보다 선호되는 방식
with open("./animals.txt", "w") as f:
    f.write("hello, world")

with open(".stars.txt", "w") as f:
    for i in range(5):
        f.write("*" * (i + 1))
        f.write("\n")
```

---

#### 함수

- 작업 자동화
- 우리가 반복적으로 사용할 어떤 특정 기능들을 재사용 가능한 코드로 작성

```python
# 함수 정의
def greeting():
    username = input()
    print("{username}님, 가입을 축하드립니다.".format(username=username))

# 함수 실행
greeting()

def print_pretty_star(count):
    """
    이 함수는 별찍기 함수입니다.
    """
    for i in range(1, count + 1):
        print("*" * (i))
# """ multi-line """ 로 내용을 적어두면 Docstring에 반영됨

print_pretty_star(5)
print_pretty_star(int(input("Count?")))

# 함수 내에 return은 함수가 반환하는 결과값을 의미 + 그 순간 함수는 종료
```

---

```python
# 함수에 어떤 숫자를 입력했을 때, 그 이하의 소수를 출력해주는 함수
# 수업 전에 내가 짠 코드
def get_prime_numbers(number):
    prime_numbers = []
    if number <= 1:
        print("소수가 없습니다.")
    else:
        for i in range(2, number + 1):
            bool = True
            for j in range(2, i):
                if i % j == 0 and i != j:
                    bool = False
                    break
            if bool == True:
                prime_numbers.append(i)
        print(prime_numbers)
```
```python
# 수업 코드

# 1차
# 우선 한 번에 모든 기능을 구현하는 것은 복잡하니,
# 각각의 과정들을 기능별로 나누어서 접근하라

# 각각의 숫자의 소수여부를 검사해서 => is_prime
# 소수면 리스트에 추가하고 최종적으로는 리스트를 출력 => get_prime_numbers

def is_prime(number):
    # 2부터 number-1 까지의 숫자를 각각 체크해본다.
    for i in range(2, number):
        if (number % i == 0):
            return False
    return True

def get_prime_numbers(number):
    prime_numbers = []
    for i in range(2, number + 1):
        if is_prime(i):
            prime_numbers.append(i)
    return prime_numbers

# 걸린 시간 측정하는 법
import time
start_time = time.time()
get_prime_numbers(1000)
end_time = time.time()
print(end_time - start_time)

# 2차 제곱근까지만 체크( 다른 방법: 홀수만 체크, 2로 나누어 떨어지는 숫자까지 체크 )
# 소수로 확인된 것의 배수를 모두 지우는 방법도 있음 => 에라토스테네스의 체
# 알고리즘에 대해서는 수학적으로 이미 증명된 것들이 매우 많기 때문에 검색해서 활용해볼 것

def is_prime_optimized(number):
    for i in range(2, int(number**0.5) + 1):
        if (number % i == 0):
            return False
    return True
```

```python
# https://goo.gl/N5QltM
# 에라토스테네스의 체 구현 코드
    #결과 값을 저장하는 목록을 미리 생성한다.
    result = []
    #전체 범위를 지정한다. : 1은 소수임을 알고 있으므로 제외하고 2부터 수행해야 하지만 초기값은 별도로 생성할 것이므로 3부터 999까지의 범위로 한다.
    candidates = range(3,1000)
    #초기값을 설정한다.
    base = 2
    #초기값의 배수를 구하기 위한 임시 변수를 생성한다.
    product = base

#전체 범위 내에서 "에라토스테네스의 체" 알고리즘을 수행한다.
    while candidates:
        #임시변수가 1000미만일 때까지 다음을 수행한다.
        while product < 1000:
            #임시변수가 전체 범위내 존재한다면 :
            if product in candidates:
                #전체 범위 목록에서 임시변수를 삭제한다.
                candidates.remove(product)
            #임시변수는 기본값의 배수이다.
            product = product+base
        #결과 목록에 기본값을 추가한다.
        result.append(base)
        #다음 기본값은 (이미 걸러진)전체 목록의 첫 번째 값이다. : 1회 걸렀을 경우 2와 2의 배수를 모두 삭제했으므로 3이다.
        base = candidates[0]
        #초기값의 배수를 구하기 위해 임시 변수를 다시 생성한다.
        product = base
        #전체 범위에서 초기값을 제거한다.
        del candidates[0]
    # 범위 내의 마지막 기본값을 결과 목록에 추가한다.
    result.append(base)
    #결과 목록을 화면에 출력한다.
    print result
```
---

##### 안수찬 선생님의 조언

- 생각하면 실행해본다, 실행력이 매우 중요
- 개발자 영어는 충분히 할 수 있는 영역
- 실력을 늘리는 가장 주요한 방법은 외주개발
- 시간을 팔아서 돈을 버는 것을 지양하자
- 코드카데미, 팀트리하우스 에서 열심히 수강해보자
- 역량을 증명하기 전까지는 후려침을 겪을 수 밖에 없다


##### 내가 개발자 성향을 가지고 있는가?

- 개발자 성향이란?
  - 문제 해결에 대한 집요함 = 문제를 중간에 포기하지 않고 끝까지 해결하려 하는가
  - 문제해결력 = 상당 부분 연습으로 극복가능


##### 파이썬 관련 추천도서

- 깐깐하게 배우는 파이썬
- 실전 파이썬 프로그래밍
- Two Scoops of Django
- 클린 코드를 위한 테스트 주도 개발
- 파이썬 라이브러리를 활용한 데이터 분석 = 데이터 분석이 필요하다면 기본서
- Udemy - Learning Python for Data Analysis and visualization


##### 왜 파이썬인가?

- 개발을 처음 배우는 이에게 가장 제대로 기본을 다질 수 있는 언어
- 파이썬을 완벽히 하는 것이 매우 중요하다, 그래야만 범용적 개발을 해나갈 수 있다
- Pandas는 R스러운 파이썬 패키지
- 모든 언어를 익힐 때 가장 먼저 배워야 할 것은 문화이다


##### 기타 팁

- jupyter 또는 ipython 은 ?변수 라고 입력하면 자세히 안내해준다
- 모르는 함수들은 늘 ?를 입력해보는 습관을 들일 것
- Google, Facebook, Jupyter 모두 Vim 단축키들이 작동함
- 파이썬은 고수준 작업에 많이 사용되다보니 비트연산을 자주 사용하지는 않음(연산자는 >> or <<)
- = 와 + 은 양쪽을 띄워줘야 한다
- 폴더명 등의 자동완성은 이용할만 하지만, 명령어 등은 자동완성하기 보다는 직접 타이핑하는 것을 권한다
- 언어의 철학에 따라 변수 지정, 패턴 등이 달라져야 한다
- 계속해서 Zen을 떠올리면서 코딩을 해야 한다
- Docker 가상 환경 = 환경만 그렇게 보이도록 한 것, 속도가 빠름
- Parallels = 가상 머신

---
