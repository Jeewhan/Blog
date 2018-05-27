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

매개변수는 지역변수와 동일한 스코프이므로 initialize 메서드가 끝나면 함께 사라져버린다, 따라서 필요한 정보를 인스턴스 변수에 저장해야 한다 (@를 붙이면 인스턴스 변수가 된다)



p는 객체의 상태를 보여주고 자기 자신을 리턴하는 것에 비해 puts는 단순히 프로그램의 표준 출력에 문자열을 출력하고 nil을 반환할 뿐이다, puts에 객체를 인자로 넘겨주면 가장 간단한 방법으로 처리한다, `#<객체의 이름:16진수로 된 고유 번호>` 하지만 만약 클래스에 to_s가 정의되어 있다면 해당 결과를 출력한다



인스턴스 변수는 각 인스턴스에 저장되며, 해당 클래스에 정의되는 모든 인스턴스 메소드에서 참조 가능하다

---

### 3.1 객체와 속성

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