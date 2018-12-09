---
layout: post
title: "Cookie || Web Storage"
categories: Dev
tags: Dev
date: 2018-12-09
---

### 쿠키, 로컬스토리지, 세션스토리지

#### Why?

우선 Cookie와 Web Storage 두 가지로 나뉘지만 그 이전에 사용해야 하는 이유가 중요할 것이다

HTTP 프로토콜 특징상 Stateless이므로 상태 정보를 유지하지 않는다

그러나 웹 애플리케이션을 만들다보면 로그인과 같이 상태 정보가 필요한 때가 있다 (세션 관리, 개인화, 트래킹)

#### How?

##### Cookie

client local에 저장되는 key/value로 이루어진 file이다

요청시 cookie가 없다면 webpage를 response로 받으면서 cookie를 client local에 저장하고, 요청시 cookie가 있다면 request에 Cookie header를 통해 cookie도 함께 전송한다

response header에 Set-Cookie 속성을 이용하면 쿠키를 만들 수 있다

단점

- 총 300개까지 cookie를 저장할 수 있고, 하나의 도메인당 20개의 값만 가질 수 있으며 하나당 4kb만 저장할 수 있다
- 매 HTTP 요청마다 포함되어 전달되므로 퍼포먼스상 좋지는 않다
- 별도의 암호화 없이 전달되므로 감청을 당할시 위험할 수 있다

##### Web Storage

cookie와 달리 서버로 자동 전송되지 않는다

local storage와 session storage의 차이는 데이터가 소멸하는 시점이다, session storage는 세션이 종료되면 즉 브라우저의 탭이 꺼지면 데이터가 사라지지만 local storage는 별도로 삭제하거나 브라우저에 의해 비활성화되지 않는다면 사용가능하다

각각 window에 포함되어있는 localStorage 와 sessionStorage 를 통해 활용할 수 있다

```javascript
localStorage.setItem('myCat', 'Tom');

sessionStorage.setItem('myCat', 'Tom');
sessionStorage.getItem("autosave")
```



참고 사이트

- [MDN HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies#세션_쿠키)
- [Web Storage API 사용하기](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)
- [쿠키와 세션의 차이, 용도, 사용법(cookie,session)](https://jeong-pro.tistory.com/80)
- [로컬스토리지, 세션스토리지 그리고 쿠키](https://isme2n.github.io/devlog/2017/06/21/storage-cookie/)
- [로컬스토리지, 세션스토리지](https://www.zerocho.com/category/HTML/post/5918515b1ed39f00182d3048)