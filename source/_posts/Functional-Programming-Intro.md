---
layout: post
title:  "실용적인 함수형 자바스크립트 Intro"
date: 2018-07-09
tags: [FP]
---
# 자바스크립트로 알아보는 함수형 프로그래밍

## 함수형 프로그래밍 개요

### 함수형 프로그래밍 정의, 순수함수

성공적인 프로그래밍이란?
- 좋은 프로그램을 만드는 일
  - 사용성, 성능, 확장성, 기획 변경에 대한 대응력이 좋은 것
- 위 사항들을 효율적이고 생산적으로 이루어지는 것이 성공적인 프로그래밍

함수형 프로그래밍은 성공적인 프로그래밍을 위해 부수효과를 미워하고 조합성을 강조하는 프로그래밍 패러다임
- 부수효과를 미워한다 => 순수함수를 만든다
  - 순수함수란?
    - 부수효과가 없음
      - 외부의 대상에 영향을 끼치지 않음
      - 인자를 변경하지 않음
      - 리턴 값 외에는 외부와 소통하는 것이 없음
    - 수학적 함수
    - 인자가 동일하면 동일한 결과를 반환
    - 상수적 자유변수를 참조할 수 없다는 것은 아님
  - 오류는 적고 안정성은 높다
- 조합성을 강조한다 => 모듈화 수준을 높인다
  - 순수함수의 조합으로 프로그래밍 진행
  - 모듈화 수준이 높다 = 생산성을 높인다
    - 모듈화 수준이 높다는 것은 성공적인 프로그래밍의 척도
    - 재사용성이 높고 팀웍도 좋고 기획 변경에 대한 대응력이 높음

순수함수란?
- 인자가 동일하면 동일한 결과를 반환
  - 평가 시점이 중요하지 않음
    - 평가시점을 개발자가 다룰 수 있음
      - 다양한 로직과 이점을 취할 수 있음
    - 항상 동일한 결과를 리턴할 것이기에 안전하고 다루기 쉬운 함수이고 따라서 조합성을 강조시킬 수 있음
      - 다른 함수의 인자로 넘겨주거나, 전혀 다른 곳에서 함수를 평가시켜도 항상 동일한 결과를 리턴
    - 그렇지 않은 함수들은 평가 시점에 따라 결과가 달라지게 됨
- 부수효과가 없음
  - 리턴 외의 출력이 없음
  - 인자를 변경하지 않음
    - 원하는 부분이 변형된 새로운 값을 만들어 리턴하는 방식으로 진행
---
### 일급함수, add_maker, 함수로 함수 실행하기

일급함수
- 함수를 값으로 다룰 수 있음
  - 변수에 담을 수 있음
  - 인자로 넘겨줄 수 있음
- 런타임에서 언제든 정의할 수 있다
- 원할 때 평가할 수 있다
  - 다른 함수가 실행하도록 할 수 있음

일급함수 개념과 순수함수 특징을 이용해서 함수의 조합성을 높여나가는 것이 함수형 프로그래밍

평가 시점에서 자유로운 순수함수들을 만들고, 순수함수들을 값으로 가지고 다니면서 적절한 시점마다 평가를 하는 다양한 로직을 만들어나가는 것이 함수형 프로그래밍

```javascript
function add_maker(a) {
  return function(b) {
    return a + b;
  }
}

var add10 = add_maker(10);

console.log( add10(20) ); // 30
```

순수함수, 일급함수와 클로저가 함께 사용되는 예제
- 내부함수는 자신의 입장에선 a를 변경하지 않고, 늘 같은 a를 바라보고 있는 순수함수
- 내부함수는 a를 참조하는 클로저
- add_maker(10)의 결과를 add10에 담을 수 있는 것은 일급함수의 성질

내 나름대로의 클로저 정의
- 자유변수를 참조하는 함수
- 스코프

```javascript
function f4(f1, f2, f3) {
  return f3(f1() + f2());
}

console.log(
  f4(
    function() { return 2; },
    function() { return 1; },
    function(a) { return a * a; }
  )
);
```

함수가 어떤 함수들을 인자로 받아서 그 함수가 원하는 로직대로 원하는 시점에 원하는 인자를 적용하면서 프로그램을 완성해나가는 것이 함수형 프로그래밍

순수함수들을 조합하고, 최종적으로 어떠한 결과를 만들어가는 것

순수함수를 만들고 조합하는 평가시점과 방법, 어떤 로직 사이에서 평가를 할 것인지 결정하면서 큰 로직을 구성

비동기, 동시성을 확보할 수 있도록 원하는 시점까지 값으로 함수를 가지고 다니다가 원하는 시점 또는 필요한 부분에서 받아둔 함수를 여러 번 실행하는 등의 로직을 구현 가능
---
### 요즘 개발 이야기, 함수형 프로그래밍 정의

요즘 개발 이야기
- 재미/실시간성 : 라이브 방송, 실시간 댓글, 협업, 메신저
- 독창성/완성도 : 애니메이션, 무한스크롤, Masonry
- 더 많아져야하는 동시성 : 비동기 I/O, CSP, Actor, STM
- 더 빨라야하는 반응성/고가용성 : ELB, Auto Scailing, OTP Supervisor
- 대용량/정확성/병렬성 : MapReduce, Clojure Reducers
- 복잡도/MSA/... : 많아지고 세밀해지는 도구들

스멀스멀 다가오는 FP
- 좋아지는 하드웨어 성능과 컴파일러, 함수형 프로그래밍 기술, 좋아지는 분산/리액티브 환경, 동시성 + 병렬성 관련 기술, 성공적인 적용 사례와 영향

마이클 포거스
- 함수형 프로그래밍은 애플리케이션, 함수의 구성요소, 더 나아가서 언어 자체를 함수처럼 여기도록 만들고, 이러한 함수 개념을 가장 우선순위에 놓는다.
- 함수형 사고방식은 문제의 해결 방법을 동사(함수)들로 구성(조합)하는 것

```javascript
// 데이터(객체) 기준
duck.moveLeft();
duck.moveRight();
dog.moveLeft();
dog.moveRight();

// 함수 기준
moveLeft(dog);
moveRight(duck);
moveLeft({ x: 5, y: 2});
moveRight(dog);
```

함수가 먼저 나오느냐, 객체가 먼저 나오느냐 (FP, OOP)
- 객체지향에서는 데이터를 먼저 디자인하고, 그 데이터에 맞는 메소드를 만드는 방식으로 진행하고, 함수형은 함수를 만들고 함수에 맞게 데이터셋을 구성하는 방식으로 진행한다 (데이터의 형태를 함수를 사용할 수 있도록 디자인)

왜? 보다는 어떻게!를 다루겠다고 하셨음, 어떻게를 알려면 어떻게 전환하는가? 전환해왔는가? 를 아는 것이 중요함
---
## 함수형으로 전환하기

### 회원 목록, map, filter

응용형 함수, 콜렉션을 다루는 함수



함수형 프로그래밍에서는 원래 있는 값을 직접 변경하지 않고 변형된 새로운 값을 리턴합니다



중복을 제거하거나 추상화할 때 함수를 이용해서 프로그래밍하는 것이 함수형 프로그래밍



조건을 인자로 올 함수에게 위임



응용형 함수, 응용형 프로그래밍, 적용형 프로그래밍

- 함수가 함수를 받아서 원하는 시점에 인자를 적용하는 형식으로 프로그래밍



고차함수

- 함수를 받거나, 함수를 리턴하거나, 인자로 받은 함수를 실행



```javascript
var tempUsers = [];

for (var i = 0; i < users.length; i++) {
  if (users[i].age >= 30) {
    tempUsers.push(users[i]);
  }
}

console.log(tempUsers);

// 프로그래머가 _filter 함수를 호출할 때, 어떤 데이터가 넘어가는지와 처리하고자 하는 조건에 대해 인지하고 있기에 재활용성이 매우 높은 함수가 되었음

// users -> list로 일반화 가능
function _filter(users, predicate) {
  var newList = [];
  
  for (var i = 0; i < users.length; i++) {
    if (predicate(users[i])) {
      newList.push(users[i]);
    }
  }
  
  return newList;
}

var names = [];

for (var i = 0; i < tempUsers.length; i++) {
  names.push(tempUsers[i].name);
}

console.log(names);

// list : 어떤 배열이든 무관, mapper : 어떤 데이터를 수집할지 위임
// 다형성이 높고, 데이터가 구체적으로 어떻게 생겼는지 보이지 않음 -> 관심사의 분리
// mapper 예시 : user => user.name
function _map(list, mapper) {
  var newList = [];
  
  for (var i = 0; i < list.length; i++) {
    newList.push(mapper(list[i]));
  }
  
  return newList;
}

console.log(
  _map(
    _filter(users, function(user) { return user.age >= 30; }),
    function(user) { return user.name; }
  )
)

// 현재 동일한 Loop를 두 번 도는 중복이 존재합니다
```



함수형 프로그래밍에서는 대입문을 많이 사용하지 않음

값을 만들어놓고 문장을 내려가면서 변형해가는 것이 아니라, 함수를 통과해가면서 한 번에 값을 새롭게 만들어가는 방식으로 함수형 프로그래밍이 진행됨

대입문이 없으면 보다 간결한 코드를 만들 수 있습니다
---
### each

```javascript
// Loop를 돌면서 적용할 로직에 대해선 인자에 올 함수에게 위임
function _each(list, iterate) {
  for (var i = 0; i < list.length; i++) {
    iterate(list[i]);
  }
  
  return list;
}

function _map(list, mapper) {
  var newList = [];
  
  _each(list, function(val) {
    newList.push(mapper(val));
  })
  
  return newList;
}

function _filter(list, predicate) {
  var newList = [];
  
  _each(list, function(val) {
    if (predicate(val)) newList.push(val);
  })
  
  return newList;
}
```

코드가 점점 간결해지고, 명령적인 코드가 숨고 선언적인 코드표현, 단순해지고 오류가 줄어들고 보다 정확하게 코딩을 하고 있다는 확신을 쉽게 느낄 수 있음
---
### 다형성

이미 자바스크립트에는 map, filter와 같은 함수들이 구현되어있는데, 우리는 왜 또다시 구현했을까?

자바스크립트에 이미 있는 그것들은 함수가 아니라 메소드이다 (순수함수 X, 객체의 상태에 따라 결과가 달라지는)

메소드는 해당 클래스의 인스턴스에서만 사용가능 (jQuery 객체나 document.querySelectorAll 의 결과인 array-like에서는 array method 사용불가) 다형성을 지원하기 어려움

함수가 기준이 되는 함수형 프로그래밍에서는 함수를 먼저 만들고 그 함수에 맞는 데이터를 구성하여 함수에게 전달하는 식으로 프로그래밍하므로 높은 다형성을 만들어내고 유용하고 실용적일 수 있게 된다

배열이 아니더라도 length property가 숫자이면 map이나 each 모두 사용가능하게 됨

함수가 먼저 나오는 프로그래밍 VS 데이터가 먼저 나오는 프로그래밍
(데이터가 먼저 나오는 프로그래밍은 데이터가 있어야만 메소드가 생길 수 있다, 그래서 객체지향에서는 평가의 순서가 대단히 중요해진다, 해당 객체가 있어야만 기능 수행 가능)

함수가 먼저 나오는 프로그래밍은 평가시점에 있어 훨씬 유연해질 수 있다 => 더 높은 조합성
(함수가 먼저 나오는 프로그래밍은 함수가 우선 먼저 홀로 존재하기 때문에 데이터가 없더라도 평가 시점이 상대적으로 훨씬 유연해진다)

응용형 함수의 장점

함수형 프로그래밍에서는 보조함수가 어떤 역할을 하느냐에 따라 굉장히 다양한 이름을 갖게 되는 점이 중요합니다

콜백함수는 어떤 일들을 수행한 뒤에 맨 끝에서 다시 돌려주는 함수만을 뜻함

predicate : 조건을 리턴하는 함수

iteratee : Loop를 돌면서 반복적으로 실행되는 함수

mapper : 무엇과 무엇을 매핑하는 함수

각각의 역할에 맞게 보조함수를 지칭해줄 것, 그렇게 할 때 다양한 좋은 응용형 고차함수들을 만들어 낼 수 있을 것


내부 다형성

외부의 다형성 (array-like도 돌릴 수 있게 되는 것)은 \_map과 같은 고차함수가 어떻게 구현되었는지에 따라 만들어지지만, 배열 안에 어떤 값이 담겨있든 수행할 수 있도록 만들어주는 내부의 다형성은 보조함수(predi, iter, mapper)가 만들어낸다

개발자가 함수에 넘겨줄 데이터에 대한 이해를 바탕으로 보조함수를 결정할 수 있고, 고차함수에서는 데이터에 대해 언급하는 코드 없이 보조함수에게 위임하기 때문에 데이터 형에 있어서 굉장히 자유롭고 다형성을 높이는데 유리합니다



> 명령형 코드인 for, if 등을 대체하는 것으로 출발해서, 메서드 내부 구현을 함수형으로 해나가는 것이 충분히 가능합니다, 또한 배열 형태의 데이터를 유사배열 객체로 디자인해서 다루거나, 클래스 내부의 본체 객체들을 함수형으로 다루는 것도 충분히 가능하며, 자바스크립트에서는 객체 지향과 함수 지향 조합이 충분히 편리합니다.
>
> 객체지향은 상태 변화가 문제가 아니라 객체지향의 해법이기 때문에 어느지점까지를 함수형으로 할지에 대한 선택이 필요하다고 생각합니다.
>
> 상태를 변경하는 일이 특정 함수나 동작의 마지막 즈음에서만 이러난다면, 좀 더 상태를 다루기 쉽지 않으실까 생각됩니다. 반대 되는 경우를 설명해보자면 객체의 특정 값을 변경해둔 상태에 의존해서 다음 메서드가 실행되어 그것에 따라 달리 동작하도록 하는식으로 코딩한다면 상대적으로 거미줄 처럼 전체 코드들이 엮이면서 관리가 어려워지기 시작합니다.
>
> 함수형과 조합할 때는 뷰를 갱신하기위한 최종 상태만 변경하는 식으로 코딩하는 것이 유리합니다. 하나씩 해보시면 감이 오실거라 생각이 됩니다.
---
### 커링, curry, curryr

커링은 함수와 인자를 다루는 기법, 함수에 인자를 하나씩 저장해나가다가 필요한 인자가 모두 채워지면 함수의 본체를 실행하는 기법, 일급함수가 지원되고 평가시점를 자유롭게 다룰 수 있으므로 구현 가능하다

커링 함수는 인자로 함수를 받고, 함수를 리턴합니다, 해당 함수는 첫 번째 인자를 받고 또 다시 함수를 리턴합니다, 인자를 모두 받으면 받았던 인자들을 미리 받아두었던 함수에 적용하면서 평가하는 함수입니다

```javascript
function _curry(fn) {
  return function(a) {
    return function(b) {
      return fn(a, b);
    }
  }
}
```

```javascript
var add = function(a, b) {
  return a + b;
}

console.log(add(10, 5)); // 15

var add = _curry(function(a, b) {
  return a + b;
});

var add10 = add(10);
console.log(add10(5));
console.log(add(5)(3));
console.log(add(10)(3));
```

본체 함수를 값으로 들고 있다가 원하는 시점까지 평가를 지연시켰다가 최종 시점에 평가하는 기법입니다

함수가 함수를 대신 실행하거나 함수가 함수를 리턴할 수 있도록 함수를 조합해나가는 방식으로 프로그래밍하는 것이 함수형 프로그래밍입니다

```javascript
function _curry(fn) {
  return function(a) {
    if (arguments.length == 2) return fn(a, b);
    return function(b) {
      return fn(a, b);
    }
  }
}

function _curry(fn) {
  return function(a, b) {
    return arguments.length == 2 ? fn(a, b) : function(b) { return fn(a, b); };
  }
}
```

```javascript
var sub = _curry(function(a, b) {
  return a - b;
})

var sub10 = sub(10);
console.log(sub10(5)); // 5
```

\_curry 만으로는 표현력이 아쉽다고 느껴지므로 오른쪽에서부터 인자를 적용해나가는 \_curryr을 만들면 좋을 것

```javascript
function _curryr(fn) {
  return function(a, b) {
    return arguments.length == 2 ? fn(a, b) : function(b) { return fn(b, a); };
  }
}

var sub = _curryr(function(a, b) {
  return a - b;
})

console.log(sub(10, 5)); // 5
var sub10 = sub(10);
console.log(sub10(5)); // -5
```

\_get : object에 있는 값을 안전하게 참조하는 함수

```javascript
function _get(obj, key) {
  return obj == null ? undefined : obj[key];
  // if (obj && obj.hasOwnProperty(key)) { return obj[key]; }
}

var user1 = { id: 1, name: "ID", age: 36 };
console.log(user1.name);
console.log(_get(user1, 'name'));

var _get = _curryr(_get);
var get_name = _get('name');
console.log(get_name(user1));

console.log(
  _map(
    _filter(users, function(user) { return user.age >= 30; }),
    _get('name') // function(user) { return user.name; }
  )
);

console.log(
  _map(
    _filter(users, function(user) { return user.age < 30; }),
    _get('age') // function(user) { return user.age; }
  )
);
```

함수를 통해서 또 다른 함수를 만들어서 map의 iteratee로 활용할 수 있다
---
### reduce

재귀적으로 연속적으로 실행한 결과를 만들어주는 함수

데이터를 받아 보조함수로 축약시킨 새로운 값을 반환, 입력된 자료구조를 축약시킨 자료구조로 만들 때 주로 사용함

map, filter 는 array로 들어온 것을 array로 리턴하기 위해서 보통 사용

```javascript
function _reduce(list, iter, memo) {
  _each(list, function(val) {
    memo = iter(memo, val);
  }
  return memo;
}

_reduce([1, 2, 3], function(a, b) {
    return a + b;
  }, 0)
);
```

복잡하거나 어려운 로직을 단순하게 구현할 수 있도록 도와주는 함수

어떻게 memo를 축약해나갈지와 데이터 모두에 보조함수를 적용한 결과를 반환하겠다는 내용이 추상화되고 선언적인 코드만 존재함

```javascript
function _reduce(list, iter, memo) {
  if (arguments.length == 2) {
    memo = list[0];
    list = list.slice(1);
  }
  _each(list, function(val) {
    memo = iter(memo, val);
  })
  return memo;
}
```

slice라는 method를 활용하게 되면 Array가 아닐 때 사용할 수 없게 됨, 아래와 같이 활용할 수도 있음

```javascript
var a = document.querySelectorAll('*');
var slice = Array.prototype.slice;
slice.call(a, 2);
slice.call(a, 2).constructor; // function Array() { [native code] }

function _rest(list, num) {
  var slice = Array.prototype.slice;
  return slice.call(list, num || 1);
}

function _reduce(list, iter, memo) {
  if (arguments.length == 2) {
    memo = list[0];
    list = _rest(list);
  }
  _each(list, function(val) {
    memo = iter(memo, val);
  })
  return memo;
}

console.log(_reduce([1, 2, 3, 4], add)); // 10
console.log(_reduce([1, 2, 3, 4], add, 10)); // 20
```

reduce는 받은 iteratee를 list의 값들에 연속적으로 적용하면서 memo라는 결과로 축약해나가는 함수
---
### 파이프라인, \_go, \_pipe, 화살표 함수

pipe는 reduce를 활용해서 만들 수 있습니다, 함수를 리턴해주는 함수인데, 함수들을 인자로 받아서 이 함수들을 연속적으로 실행해주는 함수를 리턴해주는 함수입니다

```javascript
function _pipe() {
  var fns = arguments;
  return function(arg) {
    return _reduce(fns, function(arg, fn) {
      return fn(arg);
    }, arg)
  }
}

var f1 = _pipe(
  function(a) { return a + 1; },
  function(a) { return a * 2; }
)
console.log(f1(1));
```

함수형 프로그래밍에서는 이와 같이 함수를 다루는 함수를 많이 사용합니다

pipe는 결국 reduce입니다, pipe의 보다 추상화된 레벨이 reduce입니다, pipe는 reduce를 좀 더 특화시킨 함수입니다

pipe는 함수들이라는 배열에 있는 값들을 인자에 연속적으로 적용하여 축약하는 함수라고 볼 수 있습니다

go는 pipe 함수인데 즉시실행되는 함수입니다, 첫 번째 인자로는 시작값을 받고, 두 번째 인자부터는 함수를 받아 결과를 바로 만드는 함수입니다

```javascript
function _go(arg) {
  var fns = _rest(arguments);
  return _pipe.apply(null, fns)(arg);
}

_go(1,
  function(a) { return a + 1; },
  function(a) { return a * 2; },
  console.log
)
```

그간 만든 함수들을 가지고 실제 사용을 해보도록 하겠습니다

```javascript
var users = [
  { id: 1, name: 'ID', age: 36 },
  { id: 2, name: 'BJ', age: 32 },
  { id: 3, name: 'JM', age: 32 },
  { id: 4, name: 'PJ', age: 27 },
  { id: 5, name: 'HA', age: 25 },
  { id: 6, name: 'JE', age: 26 },
  { id: 7, name: 'JI', age: 31 },
  { id: 8, name: 'MP', age: 23 },
];

console.log(
  _map(
    _filter(users, function(user) { return user.age >= 30; }),
    _get('name')
  )
)

_go(users,
  function(users) {
    return _filter(users, function(user) {
      return user.age >= 30;
    });
  },
  function(users) {
    return _map(users, _get('name');
  },
  console.log
)
```

curryr을 통해 표현력을 높여보도록 하겠습니다

```javascript
var _map = _curryr(_map), _filter = _curryr(_filter);

_map([1, 2, 3], function(val) { return val * 2}));
_map(function(val) { return val * 2})([1, 2, 3]);

_go(users,
  _filter(function(users) { return user.age >= 30; }),
  _map(_get('name')),
  console.log
)

_go(users,
  _filter(user => user.age < 30),
  _map(_get('age')),
  console.log
)
```

\_filter 함수를 만들 때, curryr을 사용해서 만들었고, \_filter에 인자를 하나만 넘겼을 때 인자가 오른쪽에서부터 적용될 또 다른 함수를 리턴하게 되고, \_go라는 함수는 함수들을 받아서 함수들을 연속적으로 실행하면서 결과를 다음 함수에게 전달하는 방식으로, 함수가 함수를 실행하고 함수가 함수를 리턴하는 방식으로 프로그래밍을 하는 것이 함수형 프로그래밍입니다

함수의 평가시점이나 인자가 적용되어가는 과정에서 하나 하나의 함수들이 모두 부수효과가 없고 순수함수들로 구성될 때 이런 조합성을 만들어낼 수 있습니다

순수함수들을 평가시점을 다루면서 조합성을 강조하는 프로그래밍, 추상화의 단위를 함수로 하는 프로그래밍을 함수형 프로그래밍이라고 합니다

화살표 함수는 아래와 같이 사용할 수 있습니다

```javascript
var a = function(user) { return user.age >= 30; };
var a = user => user.age >= 30;

var add = function(a, b) { return a + b; };
var = (a, b) => a + b;
var = (a, b) => { return a + b; };
var add = (a, b) => ({ val: a + b});
```
---
### 다형성 높이기, \_keys, 에러

다형성, 데이터 다루는 방법, 에러를 다루는 방법 등을 이야기해보도록 하겠습니다

함수형 프로그래밍에서는 예외적인 데이터가 들어오는 것에 대해 다형성을 통해 대응하기도 합니다

\_each에 null이 들어와도 에러가 나지 않도록 하는 케이스

```javascript
function _curryr(fn) {
  return function(a, b) {
    return arguments.length == 2 ? fn(a, b) : function(b) { return fn(b, a); };
  }
}

var _get = _curryr(function(obj, key) {
  return obj == null ? undefined : obj[key];
});

var _length = _get('length');

function _each(list, iter) {
  // for (var i = 0; i < list.length; i++) {
  for (var i = 0, len = _length(list); i < len; i++) {
    iter(list[i]);
  }
  return list;
}

_each(null, console.log);
console.log(_map(null, function(v) { return v; })); // []
console.log(_filter(null, function(v) { return v; })); // []
```

\_map과 \_filter 모두 \_each를 활용하고 있기에 null에 대해 적절한 다형성을 가지게 되었습니다

함수형 프로그래밍에서는 함수의 연속 실행을 할 때, 잘못된 값이 들어와도 에러가 발생하지 않고 흘려보낼 수 있도록 하는 전략을 많이 취합니다

언더스코어에서도 서로가 return하는 값이 무엇이냐와 무관하게 그럴싸한 결과값들을 내도록 코드가 많이 고려가 되어있습니다

형체크를 하지 않고 try catch를 하지 않는 이런식의 에러 처리가 불안하다고 느낄 수도 있지만, 데이터를 다루는 라이브러리들도 언더스코어나 로대시 등을 내부에서 많이 사용하고 있습니다

실용적이고 장점이 많고 에러를 내지 않고 정확하게 데이터를 다룰 수 있게 해주는 좋은 방법입니다

\_keys 만들기

```javascript
console.log(Object.keys({ name: 'ID', age: 33 })); // ["name", "age"]
console.log(Object.keys([1, 2, 3, 4])); // ["0", "1", "2", "3"]
console.log(Object.keys(10)); // []
console.log(Object.keys(null)); // Error

function _is_object(obj) {
  return typeof obj == 'object' && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

console.log(_keys(null)); // []
```

\_each 외부 다형성 높이기

```javascript
// 기존 each로는, 객체에 length가 없고, index가 0부터가 아니어서 에러 발생
_each({
  13: 'ID',
  19: 'HD',
  29: 'YD'
}, function(name) {
  console.log(name);
})

function _is_object(obj) {
  return typeof obj == 'object' && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

function _each(list, iter) {
  var keys = _keys(list); // keys는 반드시 올바른 배열을 리턴
  for (var i = 0, len = keys.length; i < len; i++) { // 따라서 length가 반드시 있을 것이라 확신할 수 있다
    iter(list[keys[i]]); // array, key/value 모두 잘 동작
  }
  return list;
}
```

함수형 프로그래밍에서는 해당하는 고차함수가 주재료로 받는 인자의 데이터 형에 따라 내부 동작을 모두 지원하도록 최대한 다형성이 높도록 코드를 구성하고, 인자가 하나만 들어오면 함수를 리턴하는 등 인자와 함수를 잘 다루는 방식으로 프로그래밍합니다, 어떤 데이터가 들어오든지 최대한 흘러갈 수 있도록 하여 연속 실행에  큰 문제가 없도록 하는 방식으로 함수를 구성합니다

형을 굉장히 강하게 체크하면서 프로그래밍하는 방식도 있지만, 이렇게 다형성을 극대화시키면서 프로그래밍하는 방식도 있습니다
---
## 컬렉션 중심 프로그래밍
### 수집하기 - map, values, pluck 등
컬렉션은 배열이나 돌림직한 데이터를 뜻한다
컬렉션을 잘 다루는 함수 세트들을 구성해나가는 프로그래밍 방식을 컬렉션 중심 프로그래밍이라고 합니다

대표적인 컬렉션을 위한 함수들을 map, filter, reduce 입니다

크게 네 가지 유형으로 나누어서 바라볼 수 있습니다
- 수집하기 - map, values, pluck 등
- 거르기 - filter, reject, compact, without 등
- 찾아내기 - find, some, every 등
- 접기 - reduce, min, max, group_by, count_by

가장 앞에 있는 함수들이 가장 추상화 레벨이 높기 때문에 대표함수라고 지칭합니다, 각 유형별 대표함수들을 활용해서 케이스별 특화함수들을 만들 수 있게 됩니다

이러한 기준으로 문제를 바라보고 해결해나가는 식으로 프로그래밍하는 것이 컬렉션 중심 프로그래밍이라고 할 수 있고, map/filter/reduce/find 등의 고차함수들을 중심으로 하는 프로그래밍이라고 볼 수 있습니다

```javascript
function _identity(val) {
  return val;
}

var a = 10;
console.log(_identity(a)); // 10

// 값을 수집하는 함수
// function _values(data) {
//   return _map(data, _identity);
// }

var _values = _map(_identity);
_values(users[0]);

console.log(users[0]); // {id: 10, name: "ID", age: 36}
console.log(_keys(users[0])); // ["id", "name", "age"]
console.log(_values(users[0])); // [10, "ID", 36]

// 배열 내부에 있는 객체에 있는 키를 통해서 꺼내진 값들을 수집하는 함수
// 내부에 있는 값을 수집한다면 _map을 활용하면 됩니다
function _pluck(data, key) {
//   return _map(data, function(obj) {
//     return obj[key];
//   });
  return _map(data, _get(key));
}

console.log(_pluck(users, 'age')); // [36, 32, 32, ...]
```
---
### 거르기 - reject, compact

reject 함수는 filter 함수를 반대로 동작시킨 함수입니다, true 평가된 값들을 제외시키는 함수입니다, 고차함수의 변경을 통해 보다 선언적으로 프로그래밍해나갈 수 있습니다

순수함수들의 평가시점들을 다루거나 함수가 함수를 리턴하거나 실행해주거나 인자로 받은 함수를 실행해준 뒤에 결과를 반대로 바꿔 리턴해준다거나 함수들의 응용과 조합을 강조하는 것이 함수형 프로그래밍입니다

```javascript
console.log(
  _filter(users, function(user) {
    return user.age > 30;
  })
)

function _negate(func) {
  return function(val) {
    return !func(val);
  }
}

// function _reject(data, predi) {
//   return _filter(data, function(val) {
//     return !predi(val);
//   })
// }

function _reject(data, predi) {
  return _filter(data, _negate(predi));
}

console.log(
  _reject(users, function(user) {
    return user.age > 30;
  })
)
```

compact 함수는 truthy한 값만 남기는 함수입니다

```javascript
var _compact = _filter(_identity);

console.log(_compact([1, 2, 0, false, null, {}])); // [1, 2, {}]
```

함수형 프로그래밍에선 함수를 간결하게 조합시켜서 약간 변경된 다양한 로직을 가진 함수들을 만드는 것을 목표로 합니다, 컬렉션 중심 프로그래밍도 마찬가지로 이러한 컬렉션을 다루는 다양하고 약간씩 다른 함수세트들을 모으는 것이 목표입니다

10개의 굉장히 복잡한 많은 기능을 하는 함수나 클래스를 만드는 것보다, 서로 다른 100개의 함수를 만드는 것이 프로그래밍 하는 것에 있어 훨씬 유리합니다
---
### 찾아내기- find, find_index, some, every

find는 조건에 해당하는 값을 처음 만났을 때 리턴해주는 함수입니다
findIndex는 해당하는 값을 처음 만났을 때 index값을 리턴해주는 함수입니다

filter 함수는 predicate를 거친 결과가 truthy한 모든 값을 수집하는 것이라면, find는 걸러지는 그 값 하나만을 리턴해주는 함수입니다

나중에 다룰 지연평가와도 연관이 있는 중요한 함수입니다

```javascript
function _find(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    var val = list[keys[i]];
    if (predi(val)) return val;
  }
}

console.log(
  _find(users, function(user) {
    return user.age < 30;
  })
)

console.log(
  _find(users, function(user) {
    return user.id == 20;
  })
)

function _find_index(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    if (predi(list[keys[i]])) return i;
  }
  return -1;
}다

console.log(
  _find_index(users, function(user) {
    return user.name == 'BJ';
  })
)

console.log(
  _get(_find_index(users, function(user) {
    return user.id == 20;
  }), 'name')
)

var _find = curryr(_find), _find_index = curryr(_find_index);

_go(users,
  _find(function(user) { return user.id == 50; }),
  _get('name'),
  console.log
);

_go(users,
  _find_index(function(user) { return user.id == 50; }),
  console.log
)
```

some은 조건에 만족하는 값이 존재하면 true를 리턴해주는 함수입니다, every는 모든 값이 조건을 만족하면 true를 리턴해주는 함수입니다
```javascript
function _some(data, predi) {
  return _find_index(data, predi) != -1;
}

function _every(data, predi) {
  return _find_index(data, _negate(predi)) == -1;
}

console.log(_some([1, 2, 5, 10, 20], function(val) {
  return val > 10;
})) // true

console.log(_every([1, 2, 5, 10, 20], function(val) {
  return val > 10;
}) // false
```

some과 every는 predi가 없어도 동작을 해야 합니다

```javascript
console.log([1, 2, 0, 10], _identity) // true
console.log(_some([null, false, 0], _identity)) // false

function _some(data, predi) {
  return _find_index(data, predi || _identity) != -1;
}

function _every(data, predi) {
  return _find_index(data, _negate(predi || _identity)) == -1;
}

console.log(_some([null, false, 0, 1])) // true

console.log(
  _some(users, function(user) {
    return user.age < 20;
  })
)
```

find, find_index, some, every 모두 고차함수로 쓰여 보조함수를 받기 때문에, 많은 것들을 해낼 수 있습니다

find_index의 경우, indexOf는 완전히 값이 같은 경우만을 해당하는 값이 몇 번째 인덱스에 있는지 찾을 수 있는 것과 달리, 보조함수를 만족하는 첫 번째 값이 몇 번째에 있는지 확인할 수 있습니다

some, every를 활용하면, 있긴 있느냐 모두 그러하냐 등도 확인할 수 있습니다

고차함수와 보조함수의 합으로 프로그래밍하는 것은 로직을 조합해나가는 것과 동일한 것입니다

하나라도 그러하느냐 => some, 그러한 조건이 무엇이냐 => predicate

이런 식으로 로직을 완성해나가는 것이 함수형 프로그래밍입니다
---
### 접기 - reduce, min_by, max_by

접기 또는 축약이라 볼 수 있습니다

Array안의 값이나 Iterable한 객체에 있는 값들을 통해서, 접혀진 값을 만들기 위해 사용합니다

순차적인 for문을 대체하는 식으로 reduce를 사용하기 보다는 함수형적인 관점에서 바라보는 것이 중요합니다

순수함수로서 평가 순서에 상관없이 접어나가는 함수로서 이해하는 것이 필요합니다

find와 달리 전체를 확인하고 특정 값으로 접어내는 함수입니다

평가순서와 상관없이 어떤 해당하는 결과를 만들 수 있는 식으로 사고하는 것이 좋습니다, 값들이 앞에서부터 순서대로 들어온다고 생각하지 않고 프로그래밍하는 것이 중요합니다, reduce라는 함수가 모든 값들을 특정 함수에게 평가시킬 것이니까 순서와 상관없도록 한 가지 로직만 생각하면서 작성하는 것입니다

reduce를 for문을 대체하는 형태로 사용한다면 그다지 함수형 프로그래밍이라고 보기 어렵고, 좋은 코드가 나오지 않을 가능성이 높습니다, 그냥 두 개의 값이 있다는 식으로 생각하면서 프로그래밍할 수 있습니다

```javascript
// _min을 작성할 때 다른 고민할 필요없이 reduce를 작성하는 것이 좋습니다

function _min(data) {
  return _reduce(data, function(a, b) {
    return a < b ? a : b; // 두 개를 비교해서 작은 값을 리턴해나가다보면 reduce에 의해 하나의 값으로 접힐 것
  });
}

function _max(data) {
  return _reduce(data, function(a, b) {
    return a > b ? a : b;
  })
}

_min([1, 2, 4, 10, 5, -4]); // -4
_max([1, 2, 4, 10, 5, -4]); // 10
```

min\_by와 max\_by는 어떤 조건을 통해서 비교를 할 것이냐를 추가적인 iteratee를 받는 것입니다, min과 max는 값을 직접 비교하는데 그래서 다형성이 상대적으로 낮습니다, 그러나 min\_by나 max\_by는 filter와 같이 보조함수를 받기 때문에 들어있는 값이 어떤 값이든지 어떤 것을 가지고 비교할지 보조함수가 한 번 더 가능성을 열어주기 때문에 더 많은 것들을 할 수 있는 min, max함수가 될 수 있습니다

예를 들어서 절대값으로의 변환 후에 비교도 가능합니다
```javascript
function _min_by(data, iter) {
  return _reduce(data, function(a, b) {
    return iter(a) < iter(b) ? a : b;
  }
}

function _max_by(data, iter) {
  return _reduce(data, function(a, b) {
    return iter(a) > iter(b) ? a : b;
  }
}

_min_by([1, 2, 4, 10, 5, -4], Math.abs);
_max_by([1, 2, 4, 10, 5, -4, -11], Math.abs);
```

상태를 변화시키지 않고 비교한다는 점이 대단히 중요한데, 만약 map을 거친 뒤에 max를 하게 되면 -11이 있을 때 -11이 나오는 것이 아니라 11이 나와버리게 됩니다, 그러면 코딩이 어려워집니다, 따라서 값을 변경시키지 않고 비교해주는 max_by와 같은 함수가 있는 것을 통해 더 쉽게 프로그래밍할 수 있게 됩니다

이런 함수적 아이디어를 통해서 계속해서 특정 부분의 다형성을 높이거나 특정 부분의 확장성을 높여서 다양한 로직들을 만들어갈 수 있게 됩니다

숫자형 데이터가 아니더라도 실무적인 데이터에도 사용할 수 있게 됩니다

```javascript
_max_by(users, function(user) { return user.age; });
_min_by(users, function(user) { return user.age; });

var _min_by = _curryr(_min_by), _max_by = _curryr(_max_by), _reject = _curryr(_reject);

_go(
  users,
  _filter(user => user.age >= 30),
  _min_by(_get('age')),
  _get('age'),
  console.log
)

_go(
  users,
  _filter(user => user.age >= 30),
  _map(_get('age')),
  _min,
  console.log
)

_go(
  users,
  _reject(user => user.age >= 30),
  _max_by(_get('age')), // max_by가 있기 때문에 나이가 아닌 user를 리턴할 수 있습니다
  _get('name'), // 추가적으로 필요한 로직 적용
  console.log
)
```
---
### 접기 - group_by, count_by
```javascript
var users = [
  { id: 10, name: 'ID', age: 36 },
  { id: 20, name: 'BJ', age: 32 },
  { id: 30, name: 'JM', age: 32 },
  { id: 40, name: 'PJ', age: 27 },
  { id: 50, name: 'HA', age: 25 },
  { id: 60, name: 'JE', age: 26 },
  { id: 70, name: 'JI', age: 31 },
  { id: 80, name: 'MP', age: 23 },
  { id: 90, name: 'FP', age: 13 },
];

// group_by 결과물
var users2 = {
  36: [{ id: 10, name: 'ID', age: 36 }],
	32: [{ id: 20, name: 'BJ', age: 32 }, { id: 30, name: 'JM', age: 32 }],
  27: [],
  ...
}

// 이 시점까지는 아무런 고민없이 코딩할 수 있습니다
// 배열을 통해 새로운 형태의 데이터를 만들 것이기 때문에 접기이고요, 그러니 reduce를 사용해야 하고, data를 주고 익명함수를 주고, 이 결과({})로 만들어나가겠다, 무엇을 조건으로 group_by를 할 것인지를 iterate에게 위임합니다
var group_by = _curryr(function(data, iter) {
  return _reduce(data, function(grouped, val) {
    
  }, {});
})

var group_by = _curryr(function(data, iter) {
  return _reduce(data, function(grouped, val) {
    var key = iter(val);
    (grouped[key] = grouped[key] || []).push(val);
    return grouped;
  }, {});
})

function _push(obj, key, val) {
  (obj[key] = obj[key] || []).push(val);
  return obj;
}

var group_by = _curryr(function(data, iter) {
  return _reduce(data, function(grouped, val) {
    return _push(grouped, iter(val), val);
  }, {});
})

_go(users,
  _group_by(_get('age')),
  console.log
);

_go(users,
  _group_by(function(user) {
    return user.age - user.age % 10;
  }),
  console.log
);

_go(users,
  _group_by(function(user) {
    return user.name[0];
  }),
  console.log
);

var _head = function(list) {
  return list[0];
}

_go(users,
  _group_by(_pipe(_get('name'), _head)),
  console.log
);
```

로직을 함수로 분리해내면 변수 선언이 줄어드는 결과를 볼 수 있습니다

group_by는 보조함수에게 어떤 기준으로 묶을 것인지를 위임하는데 이렇게 하면 유연성이 굉장히 높아져서 다양한 방식으로 group_by를 할 수 있게 됩니다

작은 로직도 함수의 연속 실행을 통해 만들어가는 프로그래밍이 함수형 프로그래밍입니다

count_by는 group_by로 만들어낸 결과에 있는 key의 개수를 리턴해주는 함수입니다, 동일한 보조함수를 넘겨주면 키에 담긴 값이 data냐 length냐의 차이만 있습니다

계속해서 이러한 접기를 만들 때, for문을 머릿 속에서 지우고 코딩을 하면 됩니다

reduce를 하면서 모든 값들을 돌면서 특정 함수를 실행시킨 결과값이 나올 것이다, 순서보다는 들어온 값에 대해서 어떤 값을 리턴해주어야 하는지를 연속적으로 했을 때 결과가 나오도록 만들면 되는 것입니다

```javascript
var _count_by = _curryr(function(data, iter) {
  return _reduce(data, function(count, val) {
    var key = iter(val);
    count[key] = count[key]++ ? count[key] = 1;
    return count;
  }, {})
});

_count_by(users, function(user) {
  return user.age;
})

_count_by(users, function(user) {
  return user.age - user.age % 10;
})

var _inc = function(count, key) {
  count[key] = count[key]++ ? count[key] = 1;
  return count;
}

var _count_by = _curryr(function(data, iter) {
  return _reduce(data, function(count, val) {
    return _inc(count, iter(val));
  }, {})
});
```

조금 더 실무적인 예제를 만들어보겠습니다

```javascript
function _each(list, iter) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    iter(list[keys[i]], keys[i]);
  }
  return list;
}

function _map(list, mapper) {
  var new_list = [];
  _each(list, function(val, key) {
    new_list.push(mapper(val, key));
  })
  return new_list;
}

_map(users[0], console.log); // 10 "id", ID name, 36 "age"

// underscore의 pairs
var pairs = _map(function(val, key) {
  return [key, val];
})

var pairs = _map((val, key) => [key, val]);

console.log(pairs(users[0]));

var _document_write = document.write.bind(document);

_go(users,
  _reject(function(user) { return user.age < 20; }),
  _count_by(function(user) { return user.age - user.age % 10; }),
  _map((count, key) => `<li>${key}대는 ${count}명 입니다.</li>`),
  list => `<ul>${list.join('')}</ul>`,
  document.write.bind(document) // this에 대한 바인딩이 되어있어야만 document.write 사용 가능
)

var f1 = _pipe(users,
  _count_by(function(user) { return user.age - user.age % 10; }),
  _map((count, key) => `<li>${key}대는 ${count}명 입니다.</li>`),
  list => `<ul>${list.join('')}</ul>`,
  document.write.bind(document) // this에 대한 바인딩이 되어있어야만 document.write 사용 가능
);

f1(users);

var f2 = _pipe(_reject(user => user.age < 20), f1);

f2(users);
_go(users, _reject(user => user.age < 20), f1);
```

어떤 로직을 만들 때, 값을 수집하는 것인지 걸러내는 것인지 특정 값을 찾아내는 것인지, 접는 것인지에 대한 유형을 먼저 분석한 다음에 프로그래밍을 해나가는 것이 좋습니다

그러면 문제를 보다 잘 정리된 패턴으로 분석을 할 수 있고 과제가 쉬워져서 프로그래밍이 좀 더 쉽게 이루어지고, i의 변화나 for문 등이 없고, 모든 함수들이 다형성이 있으면서도 안정성이 확보된 함수들을 조립하면서 프로그래밍을 하기 때문에 내가 만든 이 로직이 잘 돌아갈 것이라는 확신을 좀 더 빨리 얻을 수 있고, 테스트도 훨씬 쉽습니다

이미 테스트가 잘 된 함수들의 조합을 통해서 프로그래밍을 해나가면 보다 빠르게 이 코드가 잘 동작할 것이라는 확신을 가지면서 쉽게 함수조합을 해나갈 수 있습니다

---

## 자바스크립트에서의 지연 평가
### 지연 평가

지연 평가를 시작시키고 유지시키는(이어 가는) 함수
- map, filter, reject

지연 평가를 끝내는 함수
- take, some, every, find

```javascript
var mi = 0, fi = 0;
_.go(
  _.range(100),
  _.map(function(val) {
    ++mi;
    return val * val;
  }),
  _.filter(function(val) {
    ++fi;
    return val % 2;
  }),
  _.take(5),
  console.log
)
console.log(mi, fi); // 100, 100

var mi = 0, fi = 0;
_.go(
  _.range(100),
  L.map(function(val) {
    ++mi;
    return val * val;
  }),
  L.filter(function(val) {
    ++fi;
    return val % 2;
  }),
  L.take(5),
  console.log
)
console.log(mi, fi); // 10, 10

var mi = 0, fi = 0;
_.go(
  _.range(100),
  L.map(function(val) {
    ++mi;
    return val * val;
  }),
  L.filter(function(val) {
    ++fi;
    return val % 2;
  }),
  L.some(function(val) {
    return val > 100;
  }),
  console.log
)
console.log(mi, fi); // 12, 12
```

순수함수로 이루어져있기에 가능합니다

순수함수란 어느 시점에 평가를 해도 항상 동일한 결과를 돌려주기 때문입니다

그렇기에 내부적으로 순서를 재배치함으로서 최적화할 수 있는 여지가 생깁니다

### 요약, 클로저, 엘릭서, 병렬성

- 함수를 되도록 작게 만들기
- 다형성 높은 함수 만들기
- 상태를 변경하지 않거나 정확히 다루어 부수 효과를 최소화 하기
	- 부수효과가 없을 수는 없습니다, 다만 부수효과는 해당 로직의 최종 목적이 되니, 부수효과 전까지의 과정에서는 상태를 변경하지 않는 프로그래밍을 하다가 (과정에서는 지속적으로 부수효과를 일으키거나 상태를 변경해나가면서 프로그래밍 하는 것이 아니라) 마지막에 목적이자 결과로 부수효과를 발생시키도록 프로그래밍합니다
- 동일한 인자를 받으면 항상 동일한 결과를 리턴하는 순수 함수 만들기
- 복잡한 객체 하나를 인자로 사용하기보다 되도록 일반적인 값 여러 개를 인자로 사용하기
- 큰 로직을 고차 함수로 만들고 세부 로직을 보조 함수로 완성하기
- 어느 곳에서든 바로 혹은 미뤄서 실행할 수 있도록 (메소드가 아닌) 일반 함수이자 순수 함수로 선언하기
- 모델이나 컬렉션 등의 커스텀 객체보다는 (언어에서 지원하는 자료형인) 기본 객체를 이용하기
- 로직의 흐름을 최대한 단방향으로 흐르게 하기
- 작은 함수를 조합해서 큰 함수 만들기

아래와 같은 데이터 흐름 프로그래밍을 하기

```javascript
_go(users,
  _filter(function(user) { return user.age < 30 }),
  _map(function(user) { return user.age; }),
  console.log)

_go(users,
  _filter(user => user.age >= 30),
  _map(user => user.name),
  console.log)
```

#### 데이터 흐름 프로그래밍의 중요성

함수형 프로그래밍은 특정 언어에 국한되는 것이 아니라 언어 위에 있는 패러다임입니다
따라서 언어와 무관하게 적용 가능합니다

Clojure와 Elixer 읽기

```clojure
(def users [
  { :id 1 :name 'ID' :age 36 }
  { :id 2 :name 'BJ' :age 32 }
  { :id 3 :name 'JM' :age 34 }
  { :id 4 :name 'PJ' :age 27 }
  { :id 5 :name 'HA' :age 25 }
  { :id 6 :name 'JE' :age 26 }
  { :id 7 :name 'JI' :age 31 }
  { :id 8 :name 'MP' :age 23 }])

(println
 (map (fn [user] (:name user))
      (filter (fn [user] (< (:age user) 30)) users)))
;(PJ' HA' JE' MP')

;  console.log(
;    _map(user => user.age)
;      (_filter(user => user.age < 30)(users)));


(->> users
     (filter (fn [user] (< (:age user) 30)))
     (map (fn [user] (:name user)))
     println)
; (PJ' HA' JE' MP')

;  _go(users,
;    _filter(user => user.age < 30),
;    _map(user => user.name),
;    console.log);

(->> users
     (filter #(>= (:age %) 30))
     (map #(:name %))
     println)
;(ID' BJ' JM' JI')

(->> users
     (filter (fn [user] (< (:age user) 30)))
     (map (fn [user] (:age user)))
     println)
; (27 25 26 23)

(->> users
     (filter #(< (:age %) 30))
     (map #(:age %))
     (reduce +)
     println)
; 101


; 병렬 처리
(defn word-frequency1 [text]
  (->>
   (s/split text #"\s+")
   (r/map s/lower-case)
   (r/remove (fn [word] (s/starts-with? word "function")))
   (r/map (fn [word] { word 1 }))
   (r/fold (partial merge-with +))))

(defn -main []
  (time (word-frequency1 page))
  (time (word-frequency1 page))
  (time (word-frequency1 page))
  (time (word-frequency1 page)))
```

```elixir
defmodule Console do
    def log(v) do
        IO.puts(inspect(v))
    end
end

users = [
    %{ id: 1, name: 'ID', age: 36 },
    %{ id: 2, name: 'BJ', age: 32 },
    %{ id: 3, name: 'JM', age: 34 },
    %{ id: 4, name: 'PJ', age: 27 },
    %{ id: 5, name: 'HA', age: 25 },
    %{ id: 6, name: 'JE', age: 26 },
    %{ id: 7, name: 'JI', age: 31 },
    %{ id: 8, name: 'MP', age: 23 }
]

filtered = Enum.filter(users, fn u -> u.age >= 30 end);
Console.log(filtered);

# [%{age: 36, id: 1, name: 'ID'},
#  %{age: 32, id: 2, name: 'BJ'},
#  %{age: 34, id: 3, name: 'JM'},
#  %{age: 31, id: 7, name: 'JI'}]


Console.log(
    Enum.map(
        Enum.filter(users, fn user -> user.age >= 30 end),
        fn user -> user.name end))
# ['ID', 'BJ', 'JM', 'JI']

# console.log(
#   _map(
#     _filter(users, function(user) { return user.age >= 30; }),
#     function(user) { return user.name; }));


users
    |> Enum.filter(&(&1.age >= 30))
    |> Enum.map(&(&1.name))
    |> Console.log
    # ['ID', 'BJ', 'JM', 'JI']

#  _go(users,
#    _filter(u => u.age >= 30),
#    _map(u => u.name),
#    console.log);


users
    |> Enum.filter(&(&1.age < 30))
    |> Enum.map(&(&1.age))
    |> Console.log
    # [27, 25, 26, 23]

users
    |> Enum.filter_map(&(&1.age < 30), &(&1.age))
    |> Console.log
    # [27, 25, 26, 23]

users
    |> Enum.filter_map(&(&1.age < 30), &(&1.age))
    |> Enum.reduce(&(&1 + &2))
    |> Console.log
    # 121

users
    |> Enum.filter(&(&1.age < 30))
    |> Enum.map_reduce(0, fn(u, total) -> {u.age, u.age + total} end)
    |> Console.log
    # {[27, 25, 26, 23], 101}
```

#### 지연평가 + 병렬성 + 동시성

지연평가가 가능한 것은 언제 평가해도 동일한 순수함수의 조합으로 프로그래밍을 하기 때문입니다, 마찬가지로 어느 시점에 평가해도 상관없는 순수함수는 동시에 다른 쓰레드에서 평가를 해도 상관이 없습니다, 따라서 병렬성과 동시성에 있어서 굉장히 유리합니다

이 부분은 직접 강의를 들어봐야만 전달받을 수 있을 것!

#### 비동기 I/O NodeJS

뒷 수업에서 다루기로!

---
## 실전코드조각 1
### users, posts, comments

```javascript
var users = [
  { id: 101, name: 'ID' },
  { id: 102, name: 'BJ' },
  { id: 103, name: 'PJ' },
  { id: 104, name: 'HA' },
  { id: 105, name: 'JE' },
  { id: 106, name: 'JI' }
];

var posts = [
  { id: 201, body: '내용1', user_id: 101 },
  { id: 202, body: '내용2', user_id: 102 },
  { id: 203, body: '내용3', user_id: 103 },
  { id: 204, body: '내용4', user_id: 102 },
  { id: 205, body: '내용5', user_id: 101 },
];

var comments = [
  { id: 301, body: '댓글1', user_id: 105, post_id: 201 },
  { id: 302, body: '댓글2', user_id: 104, post_id: 201 },
  { id: 303, body: '댓글3', user_id: 104, post_id: 202 },
  { id: 304, body: '댓글4', user_id: 105, post_id: 203 },
  { id: 305, body: '댓글5', user_id: 106, post_id: 203 },
  { id: 306, body: '댓글6', user_id: 106, post_id: 204 },
  { id: 307, body: '댓글7', user_id: 102, post_id: 205 },
  { id: 308, body: '댓글8', user_id: 103, post_id: 204 },
  { id: 309, body: '댓글9', user_id: 103, post_id: 202 },
  { id: 310, body: '댓글10', user_id: 105, post_id: 201 }
];

// 1. 특정인의 posts의 모든 comments 거르기
_.go(
  _.filter(posts, function(post) {
    return post.user_id == 101;
  }),
  function(posts) {
    return _.filter(comments, function(comment) {
      return _.find(posts, function(post) {
        return post.id == comment.post_id;
      })
    })
  }),
  console.log
);

_.go(
  _.filter(posts, function(post) {
    return post.user_id == 101;
  }),
  _.map(function(post) {
    return post.id;
  }),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
  console.log
);

_.go(
  _.filter(posts, function(post) {
    return post.user_id == 101;
  }),
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
  console.log
);

_.go(
  _.where(posts, { user_id: 101 }),
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
  console.log
);

function posts_by(attr) {
  return _.where(posts, attr);
}

var comments_by_posts = _.pipe(
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  }
);

var f1 = _.pipe(posts_by, comments_by_posts);

console.log(f1({ user_id: 101 }));

// 2. 특정인의 posts에 comments를 단 친구의 이름들 뽑기
_.go(
  _.where(posts, { user_id: 101 }),
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
  _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user.id;
    }).name;
  }),
  _.uniq,
  console.log
);

function posts_by(attr) {
  return _.where(posts, attr);
}

_.go(
  posts_by({ users_id: 101 }),
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
  _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user.id;
    }).name;
  }),
  _.uniq,
  console.log
);

_.go(
  { users_id: 101 },
  posts_by,
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  },
  _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user.id;
    }).name;
  }),
  _.uniq,
  console.log
);

var comments_by_posts = _.pipe(
  _.pluck('id'),
  function(post_ids) {
    return _.filter(comments, function(comment) {
      return _.contains(post_ids, comment.post_id);
    });
  }
);

_.go({ user_id: 101 },
  posts_by,
  comments_by_posts,
  _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user.id;
    }).name;
  }),
  _.uniq,
  console.log
);

var f2 = _.pipe(
  f1,
  _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user.id;
    }).name;
  }),
  _.uniq);

console.log(f2({ user_id: 101}));

function user_names_by_comments(comments) {
  return _.map(comments, function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user_id;
    }).name;
  })
}

var user_names_by_comments = _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user_id;
    }).name;
  })

var f2 = _.pipe(f1, user_names_by_comments, _.uniq);

console.log(f2({ user_id: 101 }));

var comments_to_user_names = _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user_id;
    }).name;
  })

var f2 = _.pipe(f1, comments_to_user_names, _.uniq);

console.log(f2({ user_id: 101 }));

// 3. 특정인의 posts에 comments를 단 친구들 카운트 정보
_.go({ user_id: 101 },
  posts_by,
  comments_by_posts,
  _.map(function(comment) {
    return _.find(users, function(user) {
      return user.id == comment.user.id;
    }).name;
  }),
  _.count_by,
  console.log
);

var f3 = _.pipe(f1, comments_to_user_names, _.count_by);

console.log(f3({ user_id: 101 }));

// 4. 특정인이 comment를 단 posts 거르기
_.go(
  _.where(comments, { user_id: 105 }),
  _.pluck('post_id'),
  _.uniq,
  function(post_ids) {
    return _.filter(posts, function(post) {
      return _.contains(post_ids, post.id);
    });
  },
  console.log);
```
---
### 효율 높이기

이어질 로직들을 처리하기 좋게 데이터를 먼저 변형하고 진행하겠다라고 생각하면 좋습니다 (문제가 간결해집니다)

users 밑에 posts가 속해있고, 각각의 posts에는 comments가 속하도록 변경해두고 시작하면 뒷 부분에서 효율성을 획득할 수 있습니다

불변적으로 데이터를 다루는 것이 좋습니다, 보다 안전하고 많은 문제를 해결해주며 쉽게 해줍니다

값을 한 번 변경해놓으면 다른 로직에서 다시 참조할 때에도 문제가 발생할 수 있습니다

```javascript
// 5. users + posts + comments (index_by와 group_by로 효율 높이기)
// function find_user_by_id(user_id) { _.find(users, function(user) { return user.id == comment.user_id; }) }
// function find_user_by_id(user_id) { return user2[user_id] }
var users2 = _.index_by(users, 'id'); // 배열을 주어진 키를 기준으로 인덱싱한 새로운 객체를 만들어 리턴해주는 함수

var comments2 = _.go(
  comments,
  _.map(function(comment) {
    return _.extend({ // 값 복사해주는 함수
      // user: _.find(users, function(user) { return user.id == comment.user_id; })
      // user: find_user_by_id(comment.user_id)
      user: users2[comment.user_id]
    }, comment);
  }),
  _.group_by('post_id')); // 해당하는 key를 기준으로 배열을 담은 새로운 객체를 만들어 리턴해주는 함수

console.log(comments2);

var posts2 = _.go(
  posts,
  _.map(function(post) {
    return _.extend({
      // comments: _.filter(comments2, function(comment) { return comment.post_id == post.id }),
      comments: comments2[post.id] || [],
      user: users2[post.user_id]
    }, post);
  }));

var posts3 = _.group_by(posts2, 'user_id');
console.log(posts3);

/*
 * _.each(users2, function(user) {
 *   user.posts. = _.filter(posts2, function(post) {
 *     return post.user_id == user.id;
 *   })
 * });
 * 위와 같이 변경하면 안에 담긴 값들이 계속해서 서로 연결되는 형태가 됩니다
 * 그렇게 되면 계속해서 재귀가 되는 형태가 되므로 JSON.stringify가 불가능합니다
 */

/*
 * var users3 = _.map(users2, function(user) {
 *   return _.extend({
 *     posts: posts2[user.id] || []
 *   }, user);
 * });
 */

var users3 = _.map(users2, function(user) {
  return _.extend({
    posts: posts3[user.id] || []
  }, user);
});

console.log(users3);

// 5.1. 특정인의 posts의 모든 comments 거르기
var user = users3[0]; // 특정인 지정

_.go(user.posts,
  _.pluck('comments'),
  _.flatten, // 합쳐주는 역할
  console.log);

console.log(_.deep_pluck(user, 'posts.comments')); // 깊이있는 값을 path를 따라서 pluck + flatten을 해주는 함수

// 5.2. 특정인의 posts에 comments를 단 친구의 이름들 뽑기
_.go(user.posts,
  _.pluck('comments'),
  _.flatten,
  _.pluck('user'),
  _.pluck('name'),
  _.uniq,
  console.log);

// console.log(_.uniq(_.deep_pluck(user, 'posts.comments.user.name')));
// 함수의 중첩이 생겼으니 go로 변환한다
_.go(user, _.deep_pluck('posts.comments.user.name'), _.uniq, console.log);

// 5.3. 특정인의 posts에 comments를 단 친구들 카운트 정보

_.go(user.posts,
  _.pluck('comments'),
  _.flatten,
  _.pluck('user'),
  _.pluck('name'),
  _.count_by,
  console.log);

_.go(user, _.deep_pluck('posts.comments.user.name'), _.count_by, console.log);

// 5.4. 특정인이 comment를 단 posts 거르기

console.log(
  _.filter(posts2, function(post) {
    return _.find(post.comments, function(comment) {
      return comment.user_id == 105;
    })
  })
);
```
---
## 실전코드조각 2

reduce 등을 사용하게 되면 명령적인 코드들이 사라지므로 문제해결에만 집중할 수 있게 됩니다

```javascript
var products = [
  {
    is_selected: true, // <--- 장바구니에서 체크 박스 선택
    name: "반팔티",
    price: 10000, // <--- 기본 가격
    sizes: [ // <---- 장바구니에 담은 동일 상품의 사이즈 별 수량과 가격
      { name: "L", quantity: 4, price: 0 },
      { name: "XL", quantity: 2, price: 0 },
      { name: "2XL", quantity: 3, price: 2000 }, // <-- 옵션의 추가 가격
    ]
  },
  {
    is_selected: true,
    name: "후드티",
    price: 21000,
    sizes: [
      { name: "L", quantity: 2, price: -1000 },
      { name: "2XL", quantity: 4, price: 2000 },
    ]
  },
  {
    is_selected: false,
    name: "맨투맨",
    price: 16000,
    sizes: [
      { name: "L", quantity: 10, price: 0 }
    ]
  }
];

// 1. 모든 수량
var total_quantity = _.reduce(function(tq, product) {
  return _.reduce(product.sizes, function(tq, size) {
    return tq + size.quantity;
  }, tq);
}, 0);

_.go(products,
  total_quantity,
  console.log);

// 2. 선택 된 총 수량

_.go(products,
  _.filter(_get('is_selected')),
  total_quantity,
  console.log);

// 3. 모든 가격

var total_price = _.reduce(function(tp, product) {
  return _.reduce(product.sizes, function(tp, size) {
    return tp + (product.price + size.price) * size.quantity;
  }, tp);
}, 0);

_.go(products,
  total_price,
  console.log);

// 4. 선택 된 총 가격

_.go(products,
  _.filter(_get('is_selected')),
  total_price,
  console.log);
```

---

## 비동기

```javascript
function square(a) {
  return new Promise(function(resolve) {
    setTimeout(function() {
      resolve(a * a);
    }, 500);
  });
}

var list = [2, 3, 4];

new Promise(function(resolve) {
  (function recur(res) {
    if (list.length == res.length) return resolve(res);
    square(list[res.length]).then(function(val) {
      res.push(val);
      recur(res);
    });
  })([]);
}).then(console.log);

// 위와 동일한 결과이며, 중첩 로직을 수행해야 할 때 훨씬 쉽게 만들 수 있음
_.go(list,
  _.map(square),
//  _.map(square),
//  _.map(square),
  console.log
)

(function(w) {
  w._identity = w._idtt = function(v) { return v };
  w._noop = function() {};
  w._keys = function(obj) { return obj ? Object.keys(obj) : [] };
  w._mr = function() { return arguments._mr = true, arguments };

  w._pipe = function() {
    var fs = arguments, len = fs.length;
    return function(res) {
      var i = -1;
      while (++i < len) res = res && res._mr ? fs[i].apply(null, res) : fs[i](res);
      return res;
    }
  };

  w._go = function() {
    var i = 0, fs = arguments, len = fs.length, res = arguments[0];
    while (++i < len) res = res && res._mr ? fs[i].apply(null, res) : fs[i](res);
    return res;
  };

  w._each = function f(arr, iter) {
    if (!iter) return function(arr2) { return f(arr2, arr) };
    var i = -1, len = arr && arr.length;
    while (++i < len) iter(arr[i]);
    return arr;
  };

  w._oeach = function f(obj, iter) {
    if (!iter) return function(obj2) { return f(obj2, obj) };
    var i = -1, keys = _keys(obj), len = keys.length;
    while (++i < len) iter(obj[keys[i]]);
    return obj;
  };

  w._map = function f(arr, iter) {
    if (!iter) return function(arr2) { return f(arr2, arr) };
    var i = -1, len = arr && arr.length, res = [];
    while (++i < len) res[i] = iter(arr[i]);
    return res;
  };

  w._omap = function f(obj, iter) {
    if (!iter) return function(obj2) { return f(obj2, obj) };
    var i = -1, keys = _keys(obj), len = keys.length, res = [];
    while (++i < len) res[i] = iter(obj[keys[i]]);
    return res;
  };

  w._flatmap = w._mapcat = function f(arr, iter) {
    if (!iter) return function(arr2) { return f(arr2, arr) };
    var i = -1, len = arr && arr.length, res = [], evd;
    while (++i < len) Array.isArray(evd = iter(arr[i])) ? res.push.apply(res, evd) : res.push(evd);
    return res;
  };

  w._oflatmap = w._omapcat = function f(obj, iter) {
    if (!iter) return function(obj2) { return f(obj2, obj) };
    var i = -1, keys = _keys(obj), len = keys.length, res = [], evd;
    while (++i < len) Array.isArray(evd = iter(obj[keys[i]])) ? res.push.apply(res, evd) : res.push(evd);
    return res;
  };

  w._filter = function f(arr, iter) {
    if (!iter) return function(arr2) { return f(arr2, arr) };
    var i = -1, len = arr && arr.length, res = [];
    while (++i < len) if (iter(arr[i])) res[i].push(arr[i]);
    return res;
  };

  w._ofilter = function f(obj, iter) {
    if (!iter) return function(obj2) { return f(obj2, obj) };
    var i = -1, keys = _keys(obj), len = keys.length, res = [];
    while (++i < len) if (iter(obj[keys[i]])) res[i].push(obj[keys[i]]);
    return res;
  };

  w._reject = function f(arr, iter) {
    if (!iter) return function(arr2) { return f(arr2, arr) };
    var i = -1, len = arr && arr.length, res = [];
    while (++i < len) if (!iter(arr[i])) res[i].push(arr[i]);
    return res;
  };

  w._oreject = function f(obj, iter) {
    if (!iter) return function(obj2) { return f(obj2, obj) };
    var i = -1, keys = _keys(obj), len = keys.length, res = [];
    while (++i < len) if (!iter(obj[keys[i]])) res[i].push(obj[keys[i]]);
    return res;
  };

  w._reduce = function f(arr, iter, init) {
    if (typeof arr == "function") return function(arr2){ return f(arr2, arr, iter) };
    var i = -1, len = arr && arr.length, res = init || arr[++i];
    while (++i < len) res = iter(res, arr[i]);
    return res;
  };

  w._oreduce = function f(obj, iter, init) {
    if (typeof obj == "function") return function(obj2){ return f(obj2, obj, iter) };
    var i = -1, keys = _keys(obj), len = keys.length, res = init || obj[keys[++i]];
    while (++i < len) res = iter(res, obj[keys[i]]);
    return res;
  };

  w._find = function f(arr, iter) {
    if (!iter) return function(arr2) { return f(arr2, arr) };
    var i = -1, len = arr && arr.length;
    while (++i < len) if (iter(arr[i])) return arr[i];
  };

  w._ofind = function f(obj, iter) {
    if (!iter) return function(obj2) { return f(obj2, obj) };
    var i = -1, keys = _keys(obj), len = keys.length;
    while (++i < len) if (iter(obj[keys[i]])) return obj[keys[i]];
  };

  w._range = function(start, stop, step) {
    if (stop == null) { stop = start || 0; start = 0; }
    step = step || 1;
    var length = Math.max(Math.ceil((stop - start) / step), 0), range = Array(length);
    for (var idx = 0; idx < length; idx++, start += step) range[idx] = start;
    return range;
  };

})(typeof global == 'object' ? global : window);
```

---
## 특강

- 부수효과가 나쁜 것이 아니라, 결론에 해당함 (Just Side Effect, Not 부작용)
  - 다만 디테일하게 쫒아가면서 다루지 않으면, 에러를 유발하기가 매우 쉬움
  - 부원인, 부작용
  - 부수효과를 일으키는 대상을 리턴하도록 짜면 조합성을 높이는데 큰 도움이 됨
- 참조 투명성
- 일급함수와 클로저가 함수형 프로그래밍을 지탱하는 두 축
  - 일급함수
    - 함수를 변수, 인자, 리턴, 평가(=실행)
  - 클로저
    - 값과 변수, 함수를 통칭해서 클로저라고 함
    - 호출하여 할당함으로서 클로저가 됨
    - 메모리에서 자유변수를 유지시켜야만 클로저
    - 함수를 하나의 스택으로 보고, 자유변수에 대한 참조를 유지시키느냐 여부가 중요
    - 클로저를 만들어두고 계속해서 사용하는 패턴이 함수형 프로그래밍에서 흔함



순수함수

부수효과

필수 부수효과

새로운 값을 리턴하는 순수함수

고차함수(함수를 값으로 다루는 함수)

- 함수를 리턴해주는 함수

- 함수를 인자로 받아서 안에서 실행하는 함수 (응용형 프로그래밍)
  - function repeat(count, fn) { var i = 0; while (count--) fn(i++); }
  - var i = -1; while (++i < count) fn(i);

어떻게 동작하느냐(명령형 프로그래밍)가 아니라 이렇게 되도록 하는 것을 선언형 프로그래밍(함수형 프로그래밍 포함)

부수효과를 어떻게 다루느냐가 함수형 프로그래밍의 성향 (언어에 따라 부수효과를 모나드로 관리하기도 함)

브라우저 또는 DB를 조작해야 하므로 자바스크립트에서는 부수효과가 없는 프로그래밍은 불가능할 것 (로직 과정에선 없을 수 있음)

5종류 함수를 적절히 조합, 문보다는 함수(표현식)를 위주로 프로그래밍, 변수 사용을 줄이고 값을 변경하지 않는다, 꼭 필요한 부수 효과 함수를 제외하고는 부수 효과를 로직에 이용하지 않는다.

```javascript
function log(val) {
  console.log.apply(null, arguments);
  return val;
}
```

함수형 프로그래밍

- 결과를 리턴해주는 식으로 외부와 소통한다
- 기존 값을 수정하지 않고, 새로운 값을 만들어서 리턴한다
- 조건을 함수로 추상화하여 인자로 받는다
  - predicate(products[i]) : 로직을 완전히 위임
  - while (++i < l) predicate(list[i]) && new_list.push(list[i]))
- 추상화의 단위가 함수
  - 객체지향은 객체 또는 메소드 단위
- 표현식만으로 코딩하는 습관
- 데이터가 먼저인 프로그래밍은 다형성이 낮을 수 밖에 없음, 특정 데이터에 종속됨
  - 함수형은 함수를 먼저 만들고 함수에 맞는 형태의 데이터를 전달
  - 외부의 값에 대한 다형성을 높이고, 내부 값에 대한 다형성은 보조 함수로 커버

함수 이름은 자세히 써주고, 변수는 한 글자 수준으로 축약하곤 함, k key l length v value f func

```javascript
function add_all(list) {
  var i = 0, l = list.length, memo = list[i++];
  while (i < l) memo = add(memo, list[i++]);
  return memo;
}
```

reduce란 순회하면서 보조함수로 축약하는 것

- reduce([1,2,3,4], add)
- function add_all(list) { return reduce(list, add); }

고차함수를 만들고 적용가능한 보조함수를 전달하여 새로운 값을 만들어가는 것

상수인 변수와 함수를 가지고 새로운 상수를 만들어냄

reduce도 클로저 위에 쌓여있음

순수함수이기에 내가 원하는 기능을 한다면 얼마든지 가져다써도 괜찮고, 필요한 경우에만 만들어가면서 쓰면 될 것

자바스크립트는 Array-Like에 지원하는 함수가 많다는 것이 조금 부족한 점 (엘릭서 등은 모든 데이터셋에 지원)

함수형 프로그래밍에서 사용하는 기본 함수들은 직접 만들어보는 것이 피가 되고 살이 됨

가능한 데이터가 함수 내에서 보이지 않을 정도로 추상화해볼 것

숫자들에서 숫자로 축약하는 것은 reduce로 충분하지만, 그것이 아니면 시작하는 외부 값을 지정해줄 수 있어야 함 (더 다형성이 높은 함수가 되기 위해서)

```javascript
    function reduce(list, fn, memo) {
      var i = 0, l = list.length, memo = memo === undefined ? list[i++] : memo;
      // while (i < l) { memo = fn(memo) }
    }
```

은닉은 도구이고 취향이지, 목적이 아니다

실용적인 클로져들을 만드는 것이 함수형 프로그래밍

```javascript
function pipe() {
  var fns = arguments;
  return function(arg) {
    return reduce(fns, function(arg, f) {
      return f(arg);
    }, arg);
  }
}
    
function go() {
      return reduce(arguments, )
    }
    
    function calr(arg, f) {
      return f(arg);
    }
```

표현식만으로 코딩하면 라인이 없어서 어려운데, go를 통해 라인을 만들 수 있음

커링 : 함수의 인자를 부분적으로 적용

함수가 함수를 만들고, 함수가 다른 함수에 적용되는 함수 등을 만들어가는 함수형 프로그래밍

for, while은 코드의 모양이 익숙한 것이지, 코드의 로직이 익숙한 것이 아님

```javascript
const filter = curryr((list, predicate) => reduce(list, (new_list, val) => predicate(val) ? append(new_list, val) : new_list ))

function append(list, val) {
  return list.push(val), list; // 좌측이 실행된 뒤에 우측이 리턴됨
}
```

함수 + 파이프라인적 사고로 진행하는 것이 중요합니다

go가 reduce로 만들어진 함수

chaining은 monad의 일종이에요, chaing은 메소드를 바꿔가면서, curry는 하나의 함수로 도달하기 위해서 인자를 하나씩 전달해주다가 인자가 전부 차면 그때 실행하는 것 (실행을 미루기 위해 쓰임)

프론트엔드에서의 비동기 : 성능과 사용성, 브라우저 렌더링이 비동기와 대단히 연관되어있음 (네이티브처럼 만들 수 있음)

백엔드에서의 비동기 : 과거의 쓰레드를 사용했었는데 500개 이상은 어려웠는데 Node.js는 2만 개까지 처리 가능함, 쓰레드는 하나만 쓰고, 비동기적으로 함수를 동시다발적으로 실행하면 됨, 과거에 비해 스케일이 커지고 다이나믹해지다보니 비동기에 대한 니즈가 급증함



비동기 상황 잘 다루기

- 리턴 값으로 소통하기
- 원하는 순서대로 함수 실행을 나열하는 법 연습
- 표현식 만으로 코딩하는 연습 => 꼬리 함수 호출 최적화
- 재귀 함수 연습

기본기

- 비동기와 무관하게 내가 원하는 로직을 일렬로 나열할 수 있도록 하는 것이 제어의 첫 걸음
- 비동기라는 것은 함수 단위로 일어납니다, 단 결국은 재귀로 돌아가는 것 (+ 추상화를 얼마나 더 하느냐)

연속적으로 내가 원하는 함수를 나열하고, 원할 때 호출



reduce를 재귀 + 유명 함수를 이용하여 Promise 제어



표현식만 남긴다, 즉시 실행함수로 담으면 문이어도 함수로 만들 수 있음



프라미스 : 비동기의 결과가 값으로 다뤄질 수 있도록 함



go, pipe, map, filter



map, map (동시성)



console.time, console.timeEnd



비동기를 동기처럼 만든다 의 한 수 위가 둘을 적절히 다룬다



FP는 램은 많이 쓰되, CPU는 적게 쓰는 편



OOP와 FP는 대척점에 있지 않다, 랩을 잘 하는게 멋있는 것, 모두 현대 프로그래밍에서 필요함, DOM 조작은 모두 객체를 다뤄야 함, 프로그래밍 언어 자체보다 더 중요한 것은 멀티플 패러다임을 잘 이해하는 개발자가 되는 것, 그러면 적절한 도구를 필요할 때 쓸 수 있게 됨, 객체 지향이 없었다면 Promise가 있을 수 없었음, FP가 비동기를 지탱하는 기술임에도 함수 기법 자체는 가볍게 훑어지는 경향이 있는데 그것을 잘 다루면 더 나은 프로그래밍을 할 수 있음



네이밍 컨벤션에 카멜 케이스를 쓰지 않는 것도 함수형과 유관? => DB의 명칭과 통일

arguments는 this와 달리, 부원인이 아니라 매개변수와 동등하게 보시는지? => 외부에 변화를 주지 않는다면 부원인이 아니다

ES6는 학습자를 배려해서 의도적으로 배제? (rest parameter, Promise.all) => 그것이기도 하지만, Promise는 모든 로직을 비동기로 처리하기에 적절한 조합을 할 수 없음

_each의 가치를 언어에서 기본적으로 지원해주는 map, call, apply, Array.from에 대항하여 어떻게 내세울 수 있을지 (_go : method chaining) => forEach는 return 이 undefined라는 큰 차이가 존재하며, 리습 계열의 컨벤션은 언어에 부족한 점이 있다면 개발자가 보완하는 방향

언어에서 기본적으로 제공해주는 것에 비해, 로직 즉 탈언어적 요소 위주의 방향으로 코딩하는 것이 우선적인 지향점으로 더 좋을지 => 그게 실력향상에도 좋음
