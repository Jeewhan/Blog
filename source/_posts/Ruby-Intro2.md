---
layout: post
title: "루비 기초"
categories: Ruby
tags: Ruby
date: 2018-05-06
---

```ruby
def hello(names)
  names.each do |name| # like JS forEach
    puts "Hello, #{name.upcase}"
      # puts == print
      # string manipulation을 할 때는 "", 그렇지 않으면 ''
      # 
  end
end

rubies = ['MRI', 'jruby', 'rubinius'] # 동적 타입이며 변수에 해당하는 타입 명시 또는 선언 키워드 불필요 (like Python)

hello(rubies)
```

- 메소드 정의 `def`
- .each do |name| 은 JS의 forEach, Python의 for-in과 같다
- do...end 의 형태를 block이라고 하며, 함수에게 넘겨줄 수 있는 덩어리이다
- .each {|name| puts "Hello, #{name.upcase}"} 라고 적어도 같은 의미가 된다
  - 한 줄이면 {...}를, 여러 줄이면 do...end를 사용한다
  - 값을 반환하는 식으로 블록을 작성하려면 {...}를, 일련의 처리를 실행하기 위해서는 do...end를 사용한다
- puts는 Python의 print이다
- puts는 nil을 return하는데, nil은 JS의 null이나 Python의 None과 같다

puts는 Python의 print와 같다

​      # string manipulation을 할 때는 "", 그렇지 않으면 ''

ipt = gets.chomp

print : 개행 X

gets : 개행 포함

gets.chomp = 개행 포함 X

```ruby
print "input your gender(성별) "
gender = gets.chomp
if gender == "male"
  puts "aha! you are male. right?"
elsif gender == "female"
  puts "aha! you are female. right?"
else
  puts "please right gender(male or female)"
end
```



```ruby
print "input your gender(성별) "
gender = gets.chomp
if gender == "male"
  puts "aha! you are male. right?"
elsif gender == "female"
  puts "aha! you are female. right?"
else
  puts "please right gender(male or female)"
end
```

```ruby
fruit = ["banana", "apple", "watermelon"]
fruit << "peach"
fruit << "pineapple"
puts fruit.sample # 임의의 한 요소
puts fruit

lotto = (1..45).to_a.sample(6)
```

- 루비의 반복문은 12가지 종류, ( [루비 프로그래밍 언어의 다양한 반복문](https://www.youtube.com/watch?v=wmJJFR1Qc_Q) ), 이는 루비의 철학인 `More than one way to do things.` 과 맞닿아있는 것으로 보인다

```ruby
10.times do |i|
  puts "#{i} Hello, World!"
end

arr = ["배터리", "비행기", "자동차"]

arr.each do |x|
  puts x
end

arr.each_with_index do |x, index|
  puts "#{index + 1}. 너는 전생에 #{x} 였을 수 있어"
end

loop do
  puts "0 to exit"
  cmd = gets.chomp
  break if cmd == "0"
end
```

```ruby
student = {
  name: "Lic",
  age: 21,
  college: "seoul"
}
puts student
puts student[:name]
puts student[:age]
puts student[:college]
```

```ruby
puts('Hello '+'world')
puts('Hello '*3)
puts('Hello'[0])
puts('Hello'[1])
puts('Hello'[2])
puts('hello world'.capitalize())
puts('hello world'.upcase())
puts('Hello world'.length())
puts('Hello world'.sub('world', 'programming'))
puts(10+5)
puts("10"+"5")

al = ['A', 'B', 'C', 'D']
puts(al.length) # 4
al.push('E')
print(al) # ["A", "B", "C", "D", "E"]                                                                     
al.delete_at(0)
print(al) # ["B", "C", "D", "E"]

i = 0
while i < 10 do
    puts('puts("Hello world '+(i*9).to_s()+'")')                                                        
    i = i + 1
end

members = ['user1', 'user2', 'user3']
for member in members do
    puts(member)
end

for item in (5..10) do
  puts(item)
end

module Module1
  module_function()
  def a()
    return 'a'
  end
end
 
module Module2
  module_function()
  def a()
    return 'B'
  end
end
 
require_relative 'Module1'
require_relative 'Module2'                                                                               
puts(module1.a())
puts(module2.a())

class Cal
  def initialize(v1,v2)                                                                                     
    @v1 = v1
    @v2 = v2
  end
  def add()
    return @v1+@v2
  end
  def subtract()
    return @v1-@v2
  end
end
c1 = Cal.new(10,10)
p c1.add()
p c1.subtract()
c2 = Cal.new(30,20)
p c2.add()
p c2.subtract()

class Class1
    def method1()
        return 'm1'                                                                                      
    end
end
c1 = Class1.new()
p c1, c1.method1()
 
class Class3 < Class1
    def method2()
        return 'm2'
    end
end
 
c3 = Class3.new()
p c3, c3.method1()
p c3, c3.method2()

class Cs
  @@count = 0
  def initialize()
    @@count = @@count + 1                                                                                
  end
  def Cs.getCount()
    return @@count
  end
end
i1 = Cs.new()
i2 = Cs.new()
i3 = Cs.new()
i4 = Cs.new()
p Cs.getCount()

class C1
  def m()
    return 'parent'
  end
end
class C2 < C1
  def m()
    return super()+' child'
  end
end
o = C2.new()
p o.m()

# test.rb
require_relative 'lib'                                                                                     
obj = Lib::A.new()
p obj.a()
 
#lib.rb
module Lib
  class A
    def a()
      return 'a'
    end
  end
end

module M1
  def m1_m
    p "m1_m"
  end
end
module M2
  def m2_m
    p "m2_m"
  end
end
class C
  include M1, M2                                                                                        
end
c = C.new()
c.m1_m()
c.m2_m()
```

**외부에서 접근이 가능한 변수를 파이썬에서는 property, 루비에서는 attribute**

루비에서는 **super는 부모 객체를 가리키지 않고 ** **super가 소속되어있는 메소드와 같은 부모객체의 메소드를 가리킨다**.



String

- to_s
- gsub
- length
- blank?

Array

- first / last
- push / pop
- each / map
- select / reject
- any? / include?

nil

- nil?
- blank?
- to_i
- to_s

Time

- strftime
- wday
- year
- month
- hour

Symbol

Hash

- keys
- values
- []

if elsif else

for

case

def

---

## Class

@를 사용하면 인스턴스 변수가 되며, 이것은 인스턴스 안에 소속된 모든 메소드 안에서 활용 가능한 변수

@@를 사용하면 클래스 변수가 되며, 클래스 전체에서 사용가능한 변수 ( 인스턴스 변수의 범위 + 메소드 밖 )



상속받을 때 부모의 method도 함께 활용하고 싶다면, 자식 method에서 super를 쓰면 된다 ( super 라인에서 부모 method가 동작 )



다중상속은 불가 ( 대신 믹스인을 활용 )



public, private

private method는 class 안에서만 호출이 가능



attr_reader ( getter )

attr_writer ( setter )

attr_accessor( getter + setter )

위 키워드들을 통해 해당 attribute를 메소드로 접근할 수 있도록 할 수 있다

```ruby

```

