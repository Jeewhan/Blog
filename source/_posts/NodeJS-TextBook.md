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

- []()

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

- [Async](https://eclatant.io/2018/12/10/2018-12-10-Async/)