---
layout: post
title: "루비 곡괭이 2장 Ruby.new"
categories: Ruby
tags: Ruby
date: 2018-05-13
---

### 2.1 객체 지향 언어 루비

메서드 호출을 메시지라고 표현하고, 메시지에는 메서드 이름과 매개 변수들이 포함됨

절대값을 구해야 할 때, `-1234.abs` 와 같이 숫자 객체에 abs 메시지를 보내서 처리하도록 요청하면 된다

모든 루비 객체가 위와 같은 형식으로 동작하며, 언어 자체가 객체 지향적 구현이 잘 되어있다고 볼 수 있다

---

### 2.2 루비 기초

한 줄에 하나의 표현식만 쓴다면 ;가 필요없다

주석은 # 로 시작한다

들여쓰기가 파이썬과 같이 강제이진 않지만, 가독성을 위해 2칸 들여쓰기를 해준다

메서드 정의에는 def 키워드를 사용하며, {} 가 아닌 아래와 같은 형식으로 표현한다

```ruby
# 괄호가 없어도 되나, 함수 선언시 ()를 사용하는 것을 권장한다
def say_goodnight(name)
  result = 'Good night, ' + name
  return result
end

puts(say_goodnight('John-Boy'))
puts say_goodnight('John-Boy')
puts say_goodnight 'John-Boy'
```

루비는 동적 타이핑 언어이므로 변수 할당시 타입에 대한 명시없이 할당하면 바로 선언이 이루어진다

괄호없이 호출이 가능하다, 하지만 특정 매개변수를 어떤 메서드 호출에 보내야 할지 우선순위를 판단하기 어려울 수 있으므로, 간단한 경우를 제외하고는 괄호를 권장한다

루비에서는 '' 와 ""도 차이가 존재한다

''는 몇 가지 예외를 제외하고는 입력값이 곧 문자열의 값이 된다

""는 \n을 줄 바꿈 문자로 변경하고, 문자열 안에 #{expression}가 있을 시 평가한 값으로 변환한다

또한 루비 메서드에서는 마지막으로 실행된 표현식의 결괏값이 반환된다

```ruby
# 괄호가 없어도 되나, 함수 선언시 ()를 사용하는 것을 권장한다
def say_goodnight(name)
  return "Good night, #{name.capitalize}"
end

puts say_goodnight('uncle')
```

지역 변수, 메서드 매개 변수, 메서드 이름은 모두 소문자 또는 \_로 시작해야 한다

전역변수는 $로, 인스턴스 변수는 @, 클래스 변수는 @@로 시작한다

인스턴스 변수는 snake_case, 클래스명은 MixedCase와 같이 PascalCase Convention을 사용한다

또한 메서드명은 ?, !, = 로 끝날 수 있다

---

### 2.3 배열과 해시

배열과 해시는 키를 이용하여 접근할 수 있는 객체 모음이다

```ruby
arr = [1, 'cat', 3.14]
puts "The first element #{arr[0]}" # The first element is 1
arr[2] = nil
puts "The first element #{arr.inspect}" # The array is now [1, "cat", nil]
```

inspect는 자기 자신을 string으로 변환한 결과값을 반환하는 메서드이다

많은 언어에서는 nil(또는 null)이 아무 객체도 아님을 의미하지만, 루비에서는 아무것도 아님을 표현하는 하나의 객체이다

단어의 배열을 만드는 또 다른 방법도 있다

```ruby
arr = %w{ ant bee cat dog elk } # ["ant", "bee", "cat", "dog", "elk"]
arr[0] # "ant"
arr[3] # "dog"
```

해시는 아래와 같다

```ruby
inst_section1 = {
  'cello' => 'string'
}

# {"cello"=>"string"}

inst_section2 = {
  :cello => 'string'
}

# {:cello=>"string"}

inst_section3 = {
  'cello': 'string'
}

# {:cello=>"string"}

p inst_section1['cello'] # "string"
p inst_section1[:cello] # nil

p inst_section2['cello'] # nil
p inst_section2[:cello] # "string"

p inst_section3['cello'] # nil
p inst_section3[:cello] # "string"
```

p 메서드는 객체의 값을 화면에 출력한다 (nil과 같은 객체도 명시적으로 출력)

:cello와 같이 생긴, 심볼이라는 것이 무엇인지는 다음 챕터에서 다루도록 할테니 우선은 특수한 문자열이라고 생각하고 진행하자

[해시 키는 문자열 보다 심볼을 사용하는 것이 좋다.](https://github.com/dalzony/ruby-style-guide/blob/master/README-koKR.md#symbols-as-keys), [변경 가능한 객체를 해시 키로 사용하지 마라.](https://github.com/dalzony/ruby-style-guide/blob/master/README-koKR.md#no-mutable-keys),  [해시 키가 심볼인 경우 루비 1.9 해시 리터럴을 사용하라.](https://github.com/dalzony/ruby-style-guide/blob/master/README-koKR.md#hash-literals) 를 참고하면 객체 선언시 inst_section3과 같은 방식으로 하는 것이 선호됨을 알 수 있다

해시에서 키와 값에는 어떠한 객체가 와도 상관없다

없는 값을 참조하면 위와 같이 nil을 반환한다

루비에서는 nil이 거짓을 의미하기에 조건문에서 활용하기 좋다

그러나 예를 들어, 기본값을 0으로 바꾸는 것이 필요하다면 아래와 같이 하면 된다
(단어를 셀 때, 키가 있는지 확인하지 않아도, 그저 값을 1 올려주면 된다)

```ruby
histogram = Hash.new(0)

histogram[:ruby] = 0
histogram[:ruby] += 1

p histogram[:ruby] # 1
p histogram["ruby"] # 0
```

---

### 2.4 심볼

심볼이란 미리 정의할 필요가 없는 동시에 유일한 값이 보장되는 상수명이다

:로 시작하며 그 다음에 이름이 온다

```ruby
inst_section = {
  cello: 'string',
  clarinet: 'woodwind',
  drum: 'percussion',
  oboe: 'woodwind',
  trumpet: 'brass',
  violin: 'string'
}

puts "An oboe is a #{inst_section[:oboe]} instrument"
# An oboe is a woodwind instrument
```

---

### 2.5 제어 구조

```ruby
today = Time.now

if today.saturday?
  puts 'Do chores around the horse'
elsif today.sunday?
  puts 'Relax'
else
  puts 'Go tto work'
end

# Go to work

if radiation > 3000
  puts 'Danger, Will Robinson'
end

# 위를 한 줄로 줄이면, 아래와 같이 된다

puts 'Danger, Will Robinson' if radiation > 3000

square = 4
while square < 1000
  square *= square
end

# 위를 한 줄로 줄이면, 아래와 같이 된다

square = 4
square *= square while square < 1000
```

---

### 2.6 정규 표현식

정규 표현식은 문자열에 매치되는 패턴을 기술하는 방법이며, 루비에서는 역시 객체이다

루비에서는 슬래시 사이에 패턴을 적으면 정규 표현식을 의미한다 `/pattern/`

Perl이나 Python을 포함하는 문자열을 찾기 위해서는 다음과 같다 `/Perl|Python/`

다음과 같이 할 수도 있다 `/P(erl|ython)/`

`/ab+c/` 는 a가 하나 오고, 1개 이상의 b가 오고, 이어서 c가 오는 문자열을 뜻한다

/\d\d:\d\d:\d\d/ # 12:34:56 형태의 시간

/Perl.\*Python/ # Perl, 0 개 이상의 문자들 , 그리고 Python

/Perl Python/ # Perl, 공백 , Python

/Perl \*Python/ # Perl, 0 개 이상의 공백 , 그리고 Python

/Perl +Python/ # Perl, 1 개 이상의 공백 , 그리고 Python

/Perl\s+Python/ # Perl, 1 개 이상의 공백 문자 , 그리고 Python

/Ruby (Perl|Python)/ # Ruby, 공백 , 그리고 Perl 이나 Python

매치 연산자 =~ 는 문자열에서 패턴이 발견되면 첫 위치, 그렇지 않으면 nil을 반환한다

```ruby
line = gets

if line =~ /Perl|Python/
  puts "Scripting language mentioned: #{line}"
end
```

정규 표현식을 활용해 치환할 수도 있다

```ruby
line = gets

newline = line.sub(/Perl/, 'Ruby') # 처음 나오는 'Perl'을 'Ruby'로 바꾼다
newerline = newline.gsub(/Python/, 'Ruby') # 모든 'Python'을 'Ruby'로 바꾼다
newestline = line.gsub(/Perl|Python/, 'Ruby') # 모든 'Perl', 'Python'을 'Ruby'로 바꾼다
```

---

### 2.7 블록과 반복자

블록은 마치 매개 변수처럼 메서드 호출과 결합할 수 있는 기능이다

콜백을 구현할 수도 있고, 코드의 일부를 함수에 넘겨주거나 반복자를 구현할 수도 있다

```ruby
{ puts 'Hello' }

do
  club.enroll(person)
  person.socialize
end
```

중괄호는 do/end 쌍보다 연산자 우선순위가 높다

한 줄의 블록은 중괄호로, 여러 줄의 블록은 do/end를 사용한다

```ruby
greet { puts 'Hi' }

verbose_greet('Dave', 'loyal customer') { puts 'Hi' }
```

메서드에서 yield 문을 이용하여 블록을 여러 차례 실행할 수 있다, yield 문은 yield를 포함하는 메서드에 연관된 블록을 호출하는 메서드 호출로 생각해도 무방하다

```ruby
def call_back
  puts 'Start of method' # Start of method
  yield # In the block
  yield # In the block
  puts 'End of method' # End of method
end

call_back { puts 'In the block' }
```

블록을 메서드에 결합시키는 것은 인자를 넘겨주는 것보다는 블록과 메서드 사이에서 제어권을 주고받는 코루틴으로 보아야 한다

yield 문에 인자를 적으면 블록에 매개 변수로 전달된다, 블록에서는 |params|와 같이 매개 변수의 이름을 정의한다

```ruby
def who_says_what
  yield('Dave', 'hello') # Dave says hello
  yield('Andy', 'goodbye') # Andy says goodbye
end

who_says_what { |person, phrase| puts "#{person} says #{phrase}" }
```

반복자 구현에도 쓰이는데, 반복자란 배열 등의 집합에서 구성 요소를 하나씩 반환해주는 함수이다

```ruby
animals = %w( ant bee cat dog )
animals.each { |animal| puts animal }
# ant
# bee
# cat
# dog

[ 'cat', 'dog', 'horse' ].each { |name| print name, " " }
5.times { print "*" }
3.upto(6) {|i| print i }
4.('a'..'e').each {|char| print char }
5.puts

# cat dog horse *****3456abcde
```

---

### 2.8 읽기와 쓰기

printf는 매개 변수를 특정 형식 문자열에 따라서 제어해 출력해준다

```ruby
printf("Number: %5.2f,\nString: %s\n", 1.23, "hello")
# Number: 1.23,
# String: hello
```

%5.2f 는 다섯 글자로 맞추고, 소수점 아래 두 자리만 표시한다는 의미라고 책에 나와있지만, 숫자를 수정해도 소숫점과 달리 정수 부분은 입력한 그대로 잘 나온다..

---

### 2.9 명령행 인자

명령행에서 실행할 때 넘어오는 인자에 접근하는 방법은 두 가지이다

1. ARGV

```ruby
# cmd_line.rb
puts "You gave #{ARGV.size} arguments"
p ARGV

# $ ruby cmd_line.rb ant bee cat dog
# You gave 4 arguments
# ["ant", "bee", "cat", "dog"]
```

2. ARGF

ARGF는 명령행에서 넘겨진 이름을 가진 모든 파일의 내용을 가지고 있는 I/O이다

---

### 2.10 루비 시작하기

이번 장에선 객체, 메서드, 문자열, 컨테이너, 정규 표현식, 제어문, 반복자 에 대해서 학습하였다

---

## 3장 클래스, 객체, 변수

