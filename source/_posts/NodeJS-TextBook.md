---
layout: post
title: "Node.js 교과서"
categories: node
tags: node
date: 2018-08-12
---

## 1장 Node.js 시작하기

Node.js란 무엇인가?

Node.js, JavaScript Runtime Environment

런타임은 특정 언어로 만든 프로그램들을 실행할 수 있는 환경

따라서 는 자바스크립트를 웹 브라우저 밖에서 실행할 수 있게 해주는 환경

Node.js는 두 가지 버전 체계를 운영함, LTS(Long Term Support) / Current

LTS의 대상이 되는 버전은 짝수버전이며, LTS 스케쥴은 여기에서 확인할 수 있다 [Link](https://github.com/nodejs/Release#release-schedule)

Node.js 설치 방법은 다양하나, 여러 가지 버전을 동시에 관리해야 한다면 [nodenv](https://github.com/nodenv/nodenv) 를 추천함

Node.js 내부 구조는 Node.js Core Library, Node.js Bindings, V8(자바스크립트 엔진), libuv(비동기 I/O) 로 되어있다고 함

Node.js는 libuv 라이브러리를 통해 이벤트 기반, 논 블록킹 I/O 모델을 차용했다고 하는데, 두 가지는 각각 어떤 의미인가?

---

이벤트 루프에 대해서는 다음 두 개념을 이해한 뒤 아티클을 읽어보는 것이 가장 좋을 것이라 생각한다

프로세스란 운영체제에서 할당하는 자원의 단위이다

스레드는 프로세스 내에서 실행되는 흐름의 단위이다, 하나의 프로세스는 스레드를 여러 개 가지며 부모 프로세스의 자원을 공유할 수 있어서 같은 메모리에 접근할 수 있다

[자바스크립트와 이벤트 루프](https://meetup.toast.com/posts/89)

---

논 블록킹 I/O에 대해 이해하기 위해서 Blocking, NonBlocking, Sync, Async 에 대해 살펴보자

Block : Flow를 막고 있는 것 / NonBlock : Sub routine이 즉시 Flow 제어권을 내놓는 것

Blocking에 대한 요구사항은 내가 만들고자 하는 애플리케이션이 속한 업계마다 다를 것, 전화통화를 처리해야 한다면 Blocking에 대한 요구사항이 대단히 높을 것, 이 외에도 브라우저는 5초 등 OS 레벨에서 제한을 두는 경우도 있다, Blocking 시에는 멈춰져있지만 OS에서 제어하는 영역 내에서 실행되는 코드들은 OS가 강제 중단이 얼마든지 가능하다, 스마트폰에서 전화가 오면 그 어떤 동작보다 우선할 수 있는 것도 동일한 원리, Window에서 blue screen이 뜨는 것은 애플리케이션을 잘못 만든 경우인 것이 다반사이고, MacOS에서는 OS에서 제어할 수 있는 영역만 쓸 수 있게 하므로 권한이 제한되어있어 그런 경우가 없어 보이는 것일 뿐이다

Blocking Function 여부는 우리가 함수에 어떤 인자를 전달하느냐에 따라 블록킹 현상이 심해지느냐에 달려있다

weakset 자료구조를 사용하면 담겨있는 값의 개수와 무관하게 값이 담겨있는지 여부를 확인하는데 소요되는 비용이 같으므로 이런 식의 접근 방법이 필요할 것

일부가 Blocking Function인 것만으로도 애플리케이션 전체가 Blocking에 빠질 수 있다, Blocking의 조합으로 인한 결과는 예측할 수 없다

안타깝게도 우리가 짜는 대부분의 로직은 흔히 Blocking이기 마련이다

엔터프라이즈 레벨을 지향한다는 것의 첫 걸음은 하나의 함수를 짤 때라도 Blocking Function이 아니도록 하는 것

배열 순회, 정렬은 배열 크기에 따라, DOM 순회는 DOM의 하위구조에 따라, 이미지 프로세싱은 이미지 크기에 따라 Blocking을 유발할 것

개발자가 되기 위해 6년에 걸쳐서 의사가 된다는 마음가짐으로 임한다면 함부로 Blocking Function을 짜지 못할 것

```javascript
const f = v => other(something(v), v * 2); f(10);
```

위와 같은 코드가 있을 때, Blocking 여부를 예측할 수 있는가?

Blocking으로 인한 에러는 로그를 찍어보기도 전에 애플리케이션이 죽기 때문에 파악하기도 대단히 어려울 것



Blocking 회피방법

- 시분한 운영체제의 동시 실행 : 순차적인 실행에 비해 컨텍스트 스위칭으로 인해 훨씬 오래 걸림
- 자바스크립트 쓰레드
  - 자바스크립트는 싱글 쓰레드라고 알려져있지만 그렇지 않다, UI와 자바스크립트를 처리하는 쓰레드가 싱글일 뿐, 하지만 현대의 대부분 OS는 UI를 변경하거나 메인 스크립트를 구동하는 쓰레드를 싱글 쓰레드로 제약한다, 그리고 그 해당 쓰레드를 감시한다
  - HTML5 스펙에는 우리가 직접 Background로 사용할 수 있는 Web Worker Thread가 존재함
  - 자바스크립트 쓰레드는 무한루프를 돌고 있고 배열을 바라보고 있다, 해당 배열에 쌓이는 명령들에 언제 실행되어야 할지에 대한 프레임이 붙는다, 루프가 한 번 돌 때마다 1틱이 진행된다, 그러므로 메인 쓰레드는 명령큐에 있는 프레임들을 실행할 뿐이고, 그 외 쓰레드들에서 명령큐에 명령들을 프레임에 따라 담아둔다, 이러한 패턴을 서스펜션 패턴이라고 부른다, 다수의 공급자 쓰레드와 소비자 쓰레드를 나누어 중간에 명령큐와 같은 쿠션층을 만들어서 공급자는 쓰기만 하고 소비자는 해당하는 프레임마다 꺼내서 읽기(소비 및 명령 수행)만 한다, 따라서 메인 쓰레드가 하나의 명령을 꺼내서 처리하는 시간이 5초 이내여야 한다, 자바스크립트에서는 기본적으로 회피방식은 타임슬라이싱을 통해 여러 쓰레드에 스프레드시키는 것



Sync, Async

Sync : Sub-routine이 값을 반환함 / Async : Sub routine이 callback을 통해 값을 반환함

Blocking과 NonBlock은 Flow상으로는 동기적으로 수행되므로 큰 혼란이 없는 반면에 Sync, Async는 Flow를 어긋나게 만드므로 이해하기 쉽지 않다



Sync 코드 예시 (Sync NonBlock 의 결과값을 조사하는 것도 NonBlock해야 한다)

```javascript
// Block
const sum = n => {
  let sum = 0;
  for (let i = 1; i <= n; i++) sum += i;
  return sum;
}
sum(100);

// NonBlock
const sum = n => {
  const result = { isComplete: false };
  requestAnimationFrame(() => {
    let sum = 0;
    for (let i = 1; i <= n; i++) sum += i;
    result.isComplete = true;
    result.value = sum;
  })
  return result;
}
const result = sum(100);
const id = setInterval(() => {
  if (result.isComplete) {
    clearInterval(id);
    console.log(result.value);
  }
}, 10);
```



Async 코드 예시 (Async라고 해서 Block을 일으키지 않는 것이 아니다, 우리의 요구사항 내에 얼마나 빨리 Flow를 반환해주는지가 중요)

```javascript
// Block
const sum = (n, f) => {
  let sum = 0;
  for (let i = 1; i <= n; i++) sum += i;
  f(sum);
}
sum(100, console.log);
console.log(123); // 출력 순서 55 -> 123

// NonBlock
const sum = n => {
  requestAnimationFrame(() => {
    let sum = 0;
    for (let i = 1; i <= n; i++) sum += i;
    f(sum);
  })
}
sum(10, console.log);
console.log(123); // 출력 순서 123 -> 55
```



위 네 가지 중에서 최악의 경우는 Async Block이다, Blocking을 일으키면서 직접적인 return도 하지 않기 때문이다, 테스트나 디버깅도 매우 어려워진다

Async의 경우 코드가 널뛰기에 가독성이 떨어지므로 Promise 등을 사용해서 chaining을 하여 Flow를 인식하기 편하도록 한다, Modern API는 Non Block시에 콜백을 리턴하는 것이 아니라 Promise를 리턴하는 것도 동일한 사유이지만, 결국 콜백을 리턴하는 것과 같은 맥락에 있다 (콜백과 달리 반쯤 제어권을 가져올 수 있다는 점에서는 분명한 차이가 존재한다)

---

## 2장 알아두어야 할 자바스크립트

### ES2015+

#### const, let

var와 달리 block scope이고 재선언이 불가능하고 hoisting이 이루어지지만 TDZ가 존재한다는 점이 중요하다

또한 let과 const는 재할당 가능 여부에 따라 차이가 존재한다

마지막으로 const 변수에 담긴 것이 Primitive가 아닌 Reference Type일 경우 Immutable하다고 볼 수 없다

어떠한 객체가 가진 주소값은 동일할지라도 해당 객체에 담긴 값들은 바뀔 수 있기 때문이다

- [1. let & const](https://jaeyeophan.github.io/2017/04/18/let-const/)
- [let, const와 블록 레벨 스코프](https://poiemaweb.com/es6-block-scope)
- [let과 const는 호이스팅 될까?](https://medium.com/korbit-engineering/let과-const는-호이스팅-될까-72fcf2fac365)

#### 템플릿 문자열

`${variable}` 형태의 string interpolation 또는 template substitution는 어렵지 않지만,  훨씬 어렵고 소중한 부분은 tagged templates이다, reduce와 결합하면 템플릿 엔진 부럽지 않게 만들 수 있다

- [Tagged templates](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals#Tagged_templates)
- [ES6 In Depth: 템플릿 문자열](http://hacks.mozilla.or.kr/2015/08/es6-in-depth-template-strings-2/)

#### 화살표 함수

단순 문법을 넘어서서 함수와 결정적 차이는 책에 나오는 this binding 이외에도 prototype와 arguments이다

인자의 정보를 받아오고 싶다면 Rest parameter를 사용하거나, scope chain을 통해 상위 함수(당연히 이 함수는 화살표 함수가 아니어야 한다)의 arguments를 참조해야 한다

```javascript
const func = (...args) => console.log(...args);
func(1, 2, 3, 4, 5);

function hoc() {
  return () => console.log(arguments);
}
hoc(1, 2, 3, 4, 5)();
```

- [화살표 함수 - prototype](https://poiemaweb.com/es6-arrow-function#42-prototype)
- [ES6 In Depth: 레스트 파라메터와 디폴트 파라메터](http://hacks.mozilla.or.kr/2015/08/es6-in-depth-rest-parameters-and-defaults/)

#### 비구조화 할당

- [ES6 In Depth: 디스트럭처링(Destructuring)](http://hacks.mozilla.or.kr/2015/09/es6-in-depth-destructuring/)

#### 프로미스

비동기 연산을 다루는 객체이다

...ing