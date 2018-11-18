---
layout: post
title: "이번 주에 검색했던 것들 #1 : 18-11-18"
categories: Dev
tags: Dev
date: 2018-11-18
---

### 자바스크립트 : Form 객체에 직접 접근

[Document.forms](https://developer.mozilla.org/ko/docs/Web/API/Document/forms)

결론 : documents.forms를 통해 현재 document에 존재하는 Form element들이 담긴 collection 을 반환받을 수 있다



### 자바스크립트 : Symbol -> String

[Symbol.prototype.description](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/description)

결론 : Math.floor를 사용하세요

```javascript
console.log(Symbol("desc").description); // "desc"
console.log(Symbol.iterator.description); // "Symbol.iterator"
```



### 자바스크립트 : Object에서 key와 value를 동시에 얻고 싶을 때

[Object.entries()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

```javascript
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]
```

주의사항 : forEach, map, filter, reduce 등과 달리 k, v 조합이다