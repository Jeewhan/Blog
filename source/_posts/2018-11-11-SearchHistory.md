---
layout: post
title: "이번 주에 검색했던 것들 #0 : 18-11-11"
categories: Dev
tags: Dev
date: 2018-11-11
---

몇 달 동안 자바스크립트 및 알고리즘 문제 풀이를 쉬고 있다가, 다시 시작했다.

생각보다 잊어버린 것들이 많았어서 좌절스럽기도 했지만, 그래도 다시 열심히 해보자.



### 자바스크립트 : Integer -> String, String(value) vs value.toString()

[Converting a value to string in JavaScript](http://2ality.com/2012/03/converting-to-string.html)

결론 : value가 undefined || null 일 수 있으니 String(value)를 사용하라



### 자바스크립트 : String -> Integer, parseInt vs Math.floor

[parseInt() doesn’t always correctly convert to integer](http://2ality.com/2013/01/parseint.html)

결론 : Math.floor를 사용하세요



### 자바스크립트 : String -> Array, split()은 기대와 다른 동작

[String.prototype.split()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)

`str.split([separator[, limit]])`, 새로운 문자열을 리턴

```javascript
"hello".split(); // ["hello"]
"hello".split(""); // ["h", "e", "l", "l", "o"]
"hello".split("", 3); // ["h", "e", "l"]
```

관련 주의사항 : [How do you get a string to a character array in JavaScript?](https://stackoverflow.com/a/34717402)

결론 : ES6를 사용해도 된다면, [...str] || Array.from(str) 을 사용하자



### 자바스크립트 : String 반복

[String.prototype.repeat()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/repeat) ES6

`str.repeat(count)`, 새로운 문자열을 리턴

count는 양의 정수여야 한다

```js
'abc'.repeat(-1);   // RangeError
'abc'.repeat(0);    // ''
'abc'.repeat(1);    // 'abc'
'abc'.repeat(2);    // 'abcabc'
'abc'.repeat(3.5);  // 'abcabcabc' (count will be converted to integer)
```



만약 반복한 결과를 Array로 돌려받고 싶다면?

[Array.prototype.fill()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) ES6

`arr.fill(value[, start[, end]])`, 부수효과를 일으키는 함수



순차적으로 증가하는 숫자가 담긴 배열이 필요하다면?

```javascript
[...Array(5 + 1).keys()].slice(1); // [1, 2, 3, 4, 5]
```



### 자바스크립트 : 배열이 특정 길이인지 여부

[Array.prototype.includes()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) ES7

`arr.includes(searchElement[, fromIndex])`

```javascript
[4, 6].includes([1, 2, 3, 4]); // true
[4, 6].includes([1, 2, 3]); // false
```



### 자바스크립트 : 배열 요소중 최대값 구하기

[Math.max()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

`Math.max([값1[, 값2[, ...]]])`

만약 인수 중 하나라도 숫자로 변환하지 못한다면 [`NaN`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/NaN)로 반환합니다.

만약 아무 요소도 주어지지 않았다면 [`-Infinity`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/-Infinity)로 반환합니다.

```javascript
Math.max.apply(null, numberArray);
Math.max(...numberArray);
```

