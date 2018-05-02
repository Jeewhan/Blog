---
layout: post
title: "Big O Notation"
subtitle: "코딩인터뷰 완전 분석"
categories: CtCI
tags: CtCI
date: 2018-03-24
---

### 시간복잡도

- 점근적 실행 시간

- 그 중에서 Big O 표기법은 점근적 시간의 상한을 뜻한다

  - 계산 복잡도를 그래프로 나타내었을 때, 최악의 경우에도 알고리즘의 계산 복잡도에 비교했을 때 같거나 그보다 높은 곳에 위치할 그래프이기에 상한이라 칭한다

  - [점근적 표기법](https://ratsgo.github.io/data%20structure&algorithm/2017/09/13/asymptotic/)

- 업계에선 Big O 표기법을 Big theta, 즉 수행 시간에 딱 맞추어 표기하려고 하는 경향이 있다

- 대부분의 알고리즘은 최악의 경우와 평균적인 경우가 같다

### 공간복잡도

- 시간복잡도와 평행선을 이룸

- 재귀 호출에 쓰이는 스택 공간도 포함됨

  - 꼬리 재귀 호출을 충족한다면 해당되지 않는다

### 상수항은 무시하라, 지배적이지 않은 항을 무시하라

- Big O 표기법은 데이터의 증가에 따른 점근적 실행 시간의 변화증가율을 나타내기 위한 것이므로 무시한다

### 여러 부분으로 이루어진 알고리즘: 덧셈 VS 곱셈

- A일을 모두 마치고 B일을 해야 한다면 A + B

- A일을 할 때마다 B일을 해야 한다면 A * B

### 상환시간

- ArrayList의 경우, 배열의 용량이 꽉 찼을 때, 기존보다 크기가 두 배 더 큰 배열을 만든 뒤, 이전 배열의 모든 원소를 새 배열로 복사한다, 이때 삽입 연산의 수행 시간은 기존의 모든 원소를 새 배열로 복사해야 하기에 O(N)이 소요된다, 그러나 배열에 가용 공간이 존재할 때 삽입 연산은 O(1)이 걸린다, 위 두 가지 경우를 포함한 전체 수행 시간을 따져볼 때 상환 시간이라는 개념을 이용한다, 최악의 경우는 가끔 발생하지만 해당 수행 시간을 나머지 경우에서 분할 상환한다는 개념이다, 위 케이스에서 배열의 크기가 2의 배수일 때 원소 삽입 연산에 O(N)이 소요된다, 이러한 N의 합은 X개의 원소를 삽입할 때 2X만큼 걸리므로 이를 분할 상환하면 삽입 한 번에 필요한 시간은 O(1)이다

### log N 수행 시간

- 대표적인 케이스는 이진 탐색이다

- 탐색할 때마다 원소의 개수가 절반씩 줄어든다면, 수행 시간은 O(log N)일 가능성이 크다

### 재귀적으로 수행 시간 구하기

```
def f(n):
    if n <= 1:
        return 1
    else:
        return f(n - 1) + f(n - 1)
```

- 위 케이스의 수행 시간은 O(2^N)이다

- 트리의 깊이가 N이고, 각 노드는 두 개의 자식 노드를 가지고 있으므로 깊이가 깊어질 때마다 이전보다 두 배 더 많이 호출하게 되기 때문이다

- 다수의 호출로 이루어진 재귀 함수에서 수행 시간은 보통 O(분기^깊이)이다, 분기란 재귀 함수가 자신을 재호출하는 횟수를 뜻함

- 위 케이스의 공간복잡도는 O(N)이다

### 예제 및 연습 문제

```python
def foo(array):
    sum = 0
    product = 1

    for i in range(len(array)):
        sum += array[i]

    for j in range(len(array)):
        product *= array[i]

    print(f"{sum}, {product}")

foo([1, 2, 3, 4, 5]) # O(N)


def printPairs(array):
    for i in range(len(array)):
        for j in range(len(array)):
            print(f"{array[i]}, {array[j]}")

printPairs([1, 2, 3, 4, 5]) # O(N^2)

# 총 수행 시간을 헤아리는 것 외에도, 위 코드가 무엇을 의미하는지 살펴보는 것을 통해 구할 수도 있다
# 위의 코드는 모든 쌍을 출력하는 코드이다, 그러므로 쌍의 총 개수인 N^2과 동일


def printUnorderedPairs(array):
    for i in range(len(array)):
        for j in range(i + 1, len(array)):
            print(f"{array[i]}, {array[j]}")

printUnorderedPairs([1, 2, 3, 4, 5]) # O(N^2)

# 반복 횟수의 합
# 코드의 의미는 전체 쌍(사각형)의 절반
# 평균을 이용 => 바깥 루프는 N번, 안쪽 루프는 평균값으로 N/2


def printUnorderedPairs2(array_a, array_b):
    for i in range(len(array_a)):
        for j in range(len(array_b)):
            if array_a[i] < array_b[j]:
                print(f"{array_a[i]}, {array_b[j]}")

printUnorderedPairs2([1, 2, 3, 4, 5], [6, 7, 8, 9, 10]) # O(A*B)

# 두 배열 각각의 크기를 알 수 없으므로, 크기를 모두 고려해야 하므로 O(A*B)이다


def printUnorderedPairs3(array_a, array_b):
    for i in range(len(array_a)):
        for j in range(len(array_b)):
            for k in range(100000):
                print(f"{array_a[i], {array_b[i]}}")

printUnorderedPairs3([1, 2, 3, 4, 5], [6, 7, 8, 9, 10]) # O(A*B)

# 100000은 상수항이므로 Big O에 영향을 미치지 못한다


# P74
def reverse(array):
    for i in range(len(array) // 2):
        other = len(array) - i - 1
        array[i], array[other] = array[other], array[i]

reverse([1, 2, 3, 4, 5]) # O(N)

# 배열의 절반만 살펴본다고 해서 (반복횟수가 N의 절반이라고 해서) Big O에 영향을 끼치는 것은 아니다


# P < N/2 일 때, O(N+P) 는 O(N)
# O(2N)은 O(N)
# O(N + logN)은 O(N)
# O(N+M)은 N과 M의 관계를 알 수 없다면 O(N+M)으로 표기해야 함


# P75
# 여러 개의 문자열로 구성된 배열이 주어졌을 때, 각각의 문자열을 먼저 정렬하고
# 그 다음에 전체 문자열을 사전순으로 다시 정렬하는 알고리즘의 수행시간은 어떠하겠는가?
# 각 문자열을 정렬 => O(NlogN)
# 모든 문자열을 정렬 => O(N*NlogN)
# 전체 문자열을 사전순으로 정렬 => O(NlogN) 추가
# 따라서 O(N^2logN + NlogN)이므로 O(N^2logN)이다 는 완전히 잘못된 분석!

# 서로 다른 N을 혼용해서 쓰니까 위와 같은 오류를 범하게 된다
# 문자열의 길이를 나타낼 때와 배열의 길이를 나타낼 때를 구분하여 사용했어야 한다

# 변수 N을 사용하지 않거나 N이 가리키는 것이 명백한 경우에만 사용하라
# 연상 가능한 이름을 사용해서 새로운 변수를 정의하라

# 가장 길이가 긴 문자열의 길이를 s라 하자
# 배열의 길이를 a라 하자
# 각 문자열을 정렬하는데 O(slogs)
# a개의 문자열이므로 O(a * slogs)

# 전체 문자열을 사전순으로 정렬해야 함, 총 a개의 문자열이 있어서 O(aloga)가
# 필요하다고 생각할 수 있지만, 문자열을 비교하는 시간도 고려해야 한다
# 두 문자열을 비교하는데 O(s)가 소요되며, 비교를 O(aloga)번 해야 하므로
# 결론적으로 O(s*aloga)가 소요됨
# 위 두 부분을 합치면 전체 시간 복잡도는 O(a*s(logs + loga))가 된다


# P76
def sumNode(node):
    if node == None:
        return 0
    return sumNode(node.left) + node.value + sum(node.right)

# 균형 이진 탐색 트리에서 모든 노드의 값을 더하는 코드
# 이진 탐색 트리라는 이유로 log가 있을 것이라 착각해선 안 된다

# 코드가 무엇을 의미하는가
# 트리의 모든 노드를 방문해서 합을 구하는 것이므로, 노드의 개수와 선형 관계
# O(N)

# 재귀호출 패턴 분석
# 재귀함수에 분기가 여러 개 존재할 때, 일반적으로 O(분기^깊이)가 된다
# 트리의 깊이는 N개에 대해 logN이다
# 분기가 2라면 2^logN이므로 O(N)이 된다


# P77
# N이 소수를 확인하려면 N의 제곱근까지만 확인해보면 된다

# 어떤 수의 제곱근이란 그 수가 갖는 약수들 중에 자신을 제외하고 가장 큰 수. 라고 볼 수 있을 것 같아요.
# 소수라는게 1과 자기자신만을 약수로 가져야 하는데 또 다른 약수를 갖게 되면 안되는거니까
# 제곱근으로도 나누어 떨어지면 소수가 될 수 없는것이죠. 또한 어떤 수를 그 수의 제곱근 이상으로
# 나누게 되면 앞서 2부터 나누었던 결과가 반복되게 됩니다. 어떤 수를 2부터 제곱근까지의 수로
# 나눈다는 것을 그 수를 자신을 제외한 제수들로 나누었을 때 가질 수 있는 몫의 최소값(2)부터
# 최대값(제곱근)까지로 나눠 보는 것이라 생각하면 이해가 빠를 것 같아요.
# 제곱근 이상의 수로 나누게 되면 제곱근으로 나누었을 때보다 작은 몫을 결과로 반환하게 되서
# 제곱근 이하의 수로 나누었던 계산들을 반복하게 됩니다. 수 25에서 제곱근 5는 25가
# 가질 수 있는 몫의 최대값입니다. 그리고 그 이상의 수로 나누었을 때는 제곱근(5) 이하의 값들을
# 몫으로 반환하기 때문에 2부터 4까지 나누는 계산과 같게 되서 의미가 없는 거 같습니다.

def isPrime(n):
	i = 2

	while i ** 2 <= n:
        if n % i == 0:
            return False
        x += 1

	return True

# 시간 복잡도는 O(√N)


# P78
def factorial(n):
    if n < 0:
        return -1
    elif n == 0:
        return 1
    else:
        return n * factorial(n-1)

# O(N)


def permutation(s, prefix=""):
    if len(s) == 0:
        print(prefix)
    else:
        for i in range(len(s)):
            rem = s[0:i] + s[i+1:]
            permutation(rem, prefix + s[i])

# O(N^2 * N!)

# 순열의 종류가 모두 출력되니, O(N!)
# 순열을 조합하기 위해서 N개의 글자만큼의 호출이 필요하므로 O(N)
# 매 for문마다 문자열을 재조립하거나, 출력하는 것과 같은 O(N) 연산 소요:


# P79
def fib(n):
    if n <= 1 return n
    return fib(n-1) + fib(n-2)

# O(2^N)

# 재귀 호출이 여러 번 발생하면 지수 시간 알고리즘일 가능성이 큼


# P80
def allFib(n):
    for i in range(n):
        print(f'{i}: {fib(i)}')

def fib(n):
    if n <= 0: return 0
    if n == 1: return 1
    return fib(n - 1) + fib(n - 2)

# O(2^N)

# fib는 O(2^N)이 맞다, 그러나 전체 걸리는 시간은 O(N*2^N)이 아니다
# 그 이유는 각각의 호출시마다 N이 변화하기 때문이다
# 등비수열의 합공식에 따라 O(2^N+1)이 되므로 O(2^N)이다

# P81
def allFib(n):
    memo = [0, 1]
    for i in range(n):
        print(f'{i}: {fib(i, memo)}')

def fib(n, memo):
    if n <= 0: return memo[0]
    elif len(memo) > n or n == 1: return memo[n]
    memo.append(fib(n-1, memo) + fib(n-2, memo))
    return memo[n]

# O(N)

# 상수 시간의 일을 N번 반복한다 (캐시값을 찾아서 더한 뒤 그 결과를 캐시 배열에 저장하고 반환)


# P82
def powers_of_2(n):
    if n < 1: return 0
    elif n == 1:
        print(n)
        return n
    else:
        prev = powers_of_2(n//2)
        curr = prev * 2
        print(curr)
        return curr

# O(logN)

# 1이 될 때까지 절반씩 나누므로 log
# 해당 코드의 목적을 생각해보면, 1부터 n사이의 모든 2의 승수를 계산하고 출력하므로
# 함수의 호출 횟수는 승수의 개수와 동일하다
# n이 커질수록 수행 시간이 어떻게 바뀌는가? n의 크기가 두 배가 될 때 호출 횟수가 한 번 증가한다
```

```python
# P83
def product(a, b):
    sum = 0
    for i in range(b):
        sum += a
	return sum

# O(N)


# P84
def power(a, b):
    if b < 0: return 0
    elif b == 0: return 1
    else: return a * power(a, b - 1)

# O(N)


def mod(a, b):
    if b <= 0: return -1
    div = a / b
    return a - div * b

# O(1)


def div(a, b):
    count = 0
    sum = b
    while sum <= a:
        sum += b
        count += 1
	return count

# O(a/b)


def sqrt(n):
    return sqrt_helper(n, 1, n)

def sqrt_helper(n, mi, ma):
    if ma < mi: return -1
    guess = (mi + ma) // 2
    if guess * guess == n: return guess
    elif guess * guess < n: return sqrt_helper(n, guess + 1, ma)
    else: return sqrt_helper(n, mi, guess - 1)

# O(logN)


# P85
def sqrt(n):
    guess = 1

    while guess * guess <= n:
        if guess * guess == n:
            return guess
        guess += 1

    return -1

# O(√N)


# 이진 탐색 트리가 균형 잡혀있지 않을 때, 원소를 찾는데 걸리는 최악의 경우
# O(N)


# 이진 트리가 이진 탐색 트리가 아닐 때 시간 복잡도
# O(N)


def copyArray(array):
    copy = []

    for value in array:
        copy = appendToNew(copy, value)

    return copy

def appendToNew(array, value):
    bigger = [0] * len(array)
    
    for i in range(len(array)):
        bigger[i] = array[i]

    bigger.append(value)
    return bigger

# O(N^2)


# P86
def sumDigits(n):
    sum = 0
    
    while n > 0:
        sum += n % 10
        n //= 10

    return sum

# O(logN)


# https://github.com/careercup/CtCI-6th-Edition/blob/master/Java/Introduction/Big_O/Q_11.java
numChars = 26

def printSortedStrings(remaining, prefix=""):
    if remaining == 0:
        if isInOrder(prefix):
            print(prefix)
    else:
        for i in range(numChars):
            c = ithLetter(i)
            printSortedStrings(remaing - 1, prefix + c)

def isInOrder(s):
    for i in range(1, len(s)):
        prev = ithLetter(s[i-1])
        curr = ithLetter(s[i])

        if prev > curr:
            return False

	return True

def ithLetter(i):
    return ?


# https://github.com/careercup/CtCI-6th-Edition/blob/master/Java/Introduction/Big_O/Q_12.java
def intersection(a, b):
    mergesort(b)
    intersect = 0
    
    for v in a:
        if binarySearch(b, x) >= 0:
            intersect += 1
            
	return intersect
```
