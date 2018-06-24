---
layout: post
title: "루비 곡괭이 3장 클래스, 객체, 변수"
categories: Ruby
tags: Ruby
date: 2018-05-27
---


루비에서 다루는 모든 것은 객체이며, 클래스에서 직간접적으로 생성할 수 있다, 이번 장에서는 클래스를 만들고 다루는 법을 살펴본다



객체지향 시스템을 설계할 때 항상 제일 먼저 해야 하는 일은 다루고자 하는 대상들의 특징을 파악하는 것이다
일반적으로 다루고자 하는 대상들이 속하는 형식(type)은 클래스로 만들어진다, 그리고 각 대상은 이 클래스의 인스턴스가 된다



헌책방 관리를 위한 클래스를 만들어보자

```ruby
class BookInStock
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
    
  def to_s
    "ISBN: #{@isbn}, price: #{@price}"
  end
end

b1 = BookInStock.new('isbn1', 3)
p b1 # #<BookInStock:0x00007f93fa9acf78 @isbn="isbn1", @price=3.0>
puts b1 # ISBN: isbn1, price: 3.0

b2 = BookInStock.new('isbn2', 3.14)
p b2 # #<BookInStock:0x00007f93fa997c90 @isbn="isbn2", @price=3.14>
puts b2 # ISBN: isbn2, price: 3.14

b3 = BookInStock.new('isbn3', '5.67')
p b3 # #<BookInStock:0x00007f93fa986738 @isbn="isbn3", @price=5.67>
puts b3 # ISBN: isbn3, price: 5.67
```

initialize 메서드는 객체의 환경을 초기화해서 이를 사용 가능한 상태로 만들어두어야 한다, 그러면 다른 메서드들에서는 이 상태를 사용한다



매개변수는 지역변수와 동일한 스코프이므로 initialize 메서드가 끝나면 함께 사라져버린다, 따라서 필요한 정보를 인스턴스 변수에 저장해야 한다 (@를 붙이면 인스턴스 변수가 된다)



p는 객체의 상태를 보여주고 자기 자신을 리턴하는 것에 비해 puts는 단순히 프로그램의 표준 출력에 문자열을 출력하고 nil을 반환할 뿐이다, puts에 객체를 인자로 넘겨주면 가장 간단한 방법으로 처리한다, `#<객체의 이름:16진수로 된 고유 번호>` 하지만 만약 클래스에 to_s가 정의되어 있다면 해당 결과를 출력한다



인스턴스 변수는 각 인스턴스에 저장되며, 해당 클래스에 정의되는 모든 인스턴스 메소드에서 참조 가능하다

---

## 3.1 객체와 속성

위의 코드대로라면 객체의 내부 상태는 각 객체 내부에 저장된 정보로 다른 객체에서는 접근할 수 없다, 일반적으로 객체의 일관성을 지키기 위한 책임이 하나의 객체에 전적으로 맡겨진다는 것은 좋은 의미이다



그러나 객체의 정보가 완전히 감춰진다면 아무 의미도 없으므로, 일반적으로 객체 외부에서 객체 상태에 접근하거나 조작하는 메서드를 별도로 정의해서 외부에서도 객체 상태에 접근 가능하도록 만들어 준다, 이렇게 노출되는 내부 부분을 객체의 속성, attribute라고 한다



```ruby
class BookInStock
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
    
  def isbn
    @isbn
  end

  def price
    @price
  end
end

book = BookInStock.new("isbn1", 12.34)
puts "ISBN = #{book.isbn}" # ISBN = isbn1
puts "Price = #{book.price}" # Price = 12.34
```

위와 같은 접근자 메서드는 매우 자주 사용되므로 루비에서는 편의 메서드를 제공한다 (루비는 메서드 마지막에 평가된 표현식의 평가 결과를 반환한다, 여기서는 인스턴스 변수의 값을 반환할 것이다)



```ruby
class BookInStock
  attr_reader :isbn, :price
  
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
end

book = BookInStock.new("isbn1", 12.34)
puts "ISBN = #{book.isbn}" # ISBN = isbn1
puts "Price = #{book.price}" # Price = 12.34
```

attr_reader는 접근자 메서드를 대신 생성해준다



:isbn 에서 심볼 표현을 볼 수 있는데, 심볼은 이름을 참조할 때 사용하기 좋다, :이 있다면 isbn이라는 이름을, :이 없다면 변수의 값 자체를 의미한다, :isbn에 대응하는 접근자 메서드에서 반환할 인스턴스 변수는 당연히 @isbn이다



루비에서 인스턴스 변수와 접근자 메서드는 분리되어 있다, attr_reader는 인스턴스 변수를 쉽게 선언할 수 있도록 만들어진 선언문이 아니다



#### 쓰기 가능한 속성

```ruby
class BookInStock
  attr_reader :isbn, :price

  def initialize(isbn, price) @isbn = isbn
    @price = Float(price)
  end

  def price=(new_price)
    @price = new_price
  end
end

book = BookInStock.new("isbn1", 33.80)
puts "ISBN = #{book.isbn}" # ISBN = isbn1
puts "Price = #{book.price}" # Price = 33.8
book.price = book.price * 0.75 # 할인가격
puts "New price = #{book.price}" # New price = 25.349999999999998
```

setter 메서드는 위와 같은 형식으로 선언되어야 하는데, 값을 대입하는 메서드만 만들고 싶다면, attr_writer를 사용하면 된다, 하지만 이런 경우는 매우 드물기 때문에 인스턴스 변수의 값을 속성으로 읽는 것과 대입하는 것 모두를 한 번에 정의해주는 attr_accessor를 제공한다



```ruby
class BookInStock
  attr_reader :isbn
  attr_accessor :price

  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
end

book = BookInStock.new("isbn1", 33.80)
puts "ISBN = #{book.isbn}" # ISBN = isbn1
puts "Price = #{book.price}" # Price = 33.8
book.price = book.price * 0.75
puts "New price = #{book.price}" # New price = 25.349999999999998
```



속성과 관련한 메서드가 단순히 인스턴스 변수를 읽거나 대입만 하는 간단한 메서드일 필요는 없다



```ruby
class BookInStock
  attr_reader :isbn
  attr_accessor :price

  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end

  def price_in_cents
    Integer(price*100 + 0.5) # 0.5를 더한 뒤 정수로 변환하여 반올림된 실제 값에 가까운 정수를 얻기 위함
  end

  def price_in_cents=(cents)
    @price = cents / 100.0
  end
end

book = BookInStock.new("isbn1", 33.80)
puts "Price = #{book.price}" # Price = 33.8
puts "Price in cents = #{book.price_in_cents}" #  Price in cents = 3380
book.price_in_cents = 1234
puts "Price = #{book.price}" # Price = 12.34
puts "Price in cents = #{book.price_in_cents}" # Price in cents = 1234
```

price_in_cents는 객체 밖에서는 객체의 속성(attribute)로 보이지만, 내부적으로 저 속성에 대응하는 인스턴스 변수는 존재하지 않는다



인스턴스 변수와 계산된 값의 차이를 숨겨서, 클래스 구현에서 내부를 보호할 수 있는 방법을 제공할 수 있다, 이는 단일 접근 법칙과 관련이 있다



#### 속성, 인스턴스 변수, 메서드

속성과 메서드의 차이는 무엇인가?



속성은 단지 메서드일 뿐이다. 속성은 getter이기도, computed value를 반환하기도, 또는 setter의 역할을 하기도 한다, 어디까지가 속성이고 어디서부터가 일반 메서드인가? 속성을 일반 메서드와 구분 짓는 차이점은 무엇일까? 사실 적당히 취향대로 골라잡으면 되지만, 아래와 같이 생각해볼 수 있다



클래스를 설계할 때는 내부적으로 어떤 상태를 가지고, 이 상태를 외부(그 클래스의 사용자)에 어떤 모습으로 노출할지 결정해야 한다, 여기서 내부 상태는 인스턴스 변수에 저장한다, 외부에 보이는 상태는 속성(attribute)이라고 부르는 메서드를 통해야만 한다, 그 밖에 클래스가 할 수 있는 모든 행동은 일반 메서드를 통해야만 한다, 이런 구분법이 아주 중요한 것은 아니지만, 그래도 객체의 외부 상태를 속성이라고 부른다면 클래스를 사용하는 사람이 우리가 만든 클래스를 어떻게 봐야 하는지에 대한 힌트를 줄 수 있을 것이다

---

## 3.2 다른 클래스와 함께 사용하기



객체지향 설계에서는 외부의 대상을 파악하고 이를 코드를 통해 클래스로 만든다, 하지만 설계상에서 클래스의 대상이 되는 또 다른 대상이 있다, 이는 외부가 아닌 내부 코드 자체에 대응하는 클래스다



예를 들어 헌책방의 CSV 데이터를 읽어 들여 여러 가지 보고서를 만들어야 한다면, CSV 데이터를 읽어 들여 통계를 내고 요약해야 한다



어떻게 통계를 내고 요약할지에 따라서 설계 방향이 결정된다, 그리고 그 답은 CSV 리더에 있다 (각각의 CSV 데이터가 어떻게 생겼을지는 앞의 BookInStock에서 정의했다)



```ruby
# book_in_stock.rb
class BookInStock
  attr_reader :isbn, :price

  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
end
```

```ruby
# csv_reaedr.rb
require 'csv'
require_relative 'book_in_stock' # require_relative을 사용하는 것은 로드하려는 파일의 위치가 로드하는 파일을 기준으로 상대 위치에 있기 때문이다. 여기서 두 파일은 모두 같은 위치에 있다

class CsvReader
  def initialize
    @books_in_stock = []
  end
    
  def read_in_csv_data(csv_file_name)
    CSV.foreach(csv_file_name, headers: true) do |row|
      @books_in_stock << BookInStock.new(row["ISBN"], row["Price"])
    end
  end
    
  def total_value_in_stock
    sum = 0.0
    @books_in_stock.each {|book| sum += book.price}
    sum
  end
    
  def number_of_each_isbn
  end
end
```

```ruby
# stock_stats.rb
require_relative 'csv_reader'

reader = CsvReader.new ARGV.each do |csv_file_name|
  STDERR.puts "Processing #{csv_file_name}"
  reader.read_in_csv_data(csv_file_name)
end

puts "Total value = #{reader.total_value_in_stock}"
```

---

## 3.3 접근 제어

클래스 인터페이스를 설계할 때, 클래스를 외부에 어느 정도까지 노출할지 결정하는 것은 중요한 일이다



클래스에 너무 깊이 접근하도록 허용하면 각 요소 간의 결합도가 높아질 우려가 있다, 다시 말해 이 클래스의 사용자 코드는 클래스 내부 구현의 세세한 부분에까지 종속적이 되기 쉽다는 것이다



루비에서 객체 상태를 변경하는 방법은 메서드를 호출하는 것일 뿐이므로 메서드에 대한 접근을 적절히 설정하면 객체에 대한 접근을 제어할 수 있다, 경험적으로 볼 때 객체의 상태를 망가뜨릴 수 있는 메서드는 노출해서는 안 된다



루비의 세 가지 보호 단계는 다음과 같다

- public : 접근 제어가 없음, 메서드는 기본적으로 public (단, initialize는 예외적으로 항상 private)
- protected : 이 메서드는 그 객체를 정의한 클래스와 하위 클래스에서만 호출할 수 있다
- private : 메서드는 수신자를 지정해서 호출할 수 없다, 오직 현재 객체에서만 호출 가능


- [ ] 루비는 접근 제어가 동적으로 결정된다, 따라서 접근 위반 예외는 제한된 메서드를 실제로 호출한 그 때에만 발생한다



### 접근 제어 기술하기

```ruby
class Account
  attr_accessor :balance
    
  def initialize(balance)
    @balance = balance
  end
end

class Transaction
  def initialize(account_a, account_b)
    @account_a = account_a
    @account_b = account_b
  end
    
  def transfer(amount)
    debit(@account_a, amount)
    credit(@account_b, amount)
  end
    
  private
  
  def debit(account, amount)
    account.balance -= amount
  end

  def credit(account, amount)
    account.balance += amount
  end
end

savings = Account.new(100)
checking = Account.new(200)
trans = Transaction.new(checking, savings)
trans.transfer(50)
```



protected 접근은 객체가 같은 클래스에서 생성된 다른 객체의 상태에 접근할 필요가 있을 때 사용한다. 예를 들어 각각의 Account 객체의 결산 잔액을 비교 하고 싶은데, 잔액 자체는 (아마 다른 형식으로 보여주고자 하기 때문에) 외부에 보여주고 싶지는 않은 경우를 보자.



```ruby
class Account
  attr_reader :cleared_balance # 접근자 메서드 'cleared_balance'를 만든다.
  protected :cleared_balance # 접근자 메서드를 protected 메서드로 설정한다.

  def greater_balance_than?(other)
    @cleared_balance > other.cleared_balance
  end
end
```

---

## 3.4 변수

```ruby
person = "Tim"
puts "The object in 'person' is a #{person.class}" # The object in 'person' is a String
puts "The object has an id of #{person.object_id}" # The object has an id of 70264079641280
puts "and a value of '#{person}'" # and a value of 'Tim'
```

변수는 객체가 아니라 객체에 대한 참조를 가지고 있을 뿐이다, 힙 메모리에 있는 객체를 변수가 가리키고 있다



```ruby
person1 = "Tim"
person2 = person1
person1[0] = 'J'
puts "person1 is #{person1}" # person1 is Jim
puts "person2 is #{person2}" # person2 is Jim
```

person2에 person1을 대입해도 새로운 객체는 생성되지 않으며, 단지 person1 객체에 대한 참조를 person2에 복사해서 같은 객체를 참조하도록 만들 뿐이다



대입은 객체의 별명을 늘려서 결과적으로 여러 개의 변수가 하나의 객체를 참조하도록 한다



dup 메서드를 사용한다면 같은 내용을 담은 객체를 새로 생성한다



```ruby
person1 = "Tim"
person2 = person1.dup person1[0] = "J"
puts "person1 is #{person1}" # person1 is Jim
puts "person2 is #{person2}" # person2 is Tim
```



객체를 동결해서 객체의 상태를 변경할 수 없도록 할 수도 있다, 동결된 객체를 수정하려고 하면 루비는 RuntimeError 예외를 발생시킨다

```ruby
person1 = "Tim"
person2 = person1
person1.freeze # 객체 수정을 막는다.
person2[0] = "J"

# 실행 결과:
# from prog.rb:4:in `<main>'
# prog.rb:4:in `[]=': can't modify frozen String (RuntimeError)
```



클래스 메서드, 믹스인, 상속 등의 개념은 