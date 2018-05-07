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

