---
layout: post
title: "코드스피츠 3기 함수와 OOP - 3강"
categories: Dev
tags: Dev
date: 2018-09-20
---

어플리케이션을 짤 때, 대부분의 경우는 인식하여 구조화해서 프로그래밍 해야 하는 경우가 대부분

HTML 파서를 짤려면 HTML 을 보고 구조적으로 분석해야 함

구조화를 무한히 하면 무한한 케이스가 될 뿐, 구조화의 목표는 단순한 원리의 조합이나 창발을 통해서 설명할 수 있고자 하는 것

A = <TAG>BODY</TAG>

B = <TAG />

C = TEXT

BODY = (A | B | C)N

위와 같이 간단한 원리로 압축하여 그것의 창발로 현상을 설명할 수 있어야, 알고리즘도 간단한 만큼만 구현할 수 있다

HTML 을 장황하게 설명하면, 장황함을 커버하는 알고리즘을 짜야 한다

현상을 보고 구조적이고 재귀적인 형태의 파악을 할 수 있는가? 데이터 구조를 만들어 낼 수 있는가? 가 핵심이다

내부 구성요소로부터 응용 구성요소로 확장하는 것을 BNF 정의방식이라고 함

A 는 BODY 를 가지고 있는데 BODY 는 A 를 가지고 있을 수 있으므로 재귀가 시작됨

재귀적인 센스가 없다면 위와 같이 사물을 바라보기가 어렵다

재귀적으로 짜지 않으면 모든 케이스를 다 처리하기 위한 조건문이 필요해진다

재귀적으로 처리하는 과제를 수행해보게 되면, 그 다음부터는 해당 계열의 문제를 해결할 수 있고 그러면 사물을 그렇게 바라볼 수 있게 되고, 그렇게 되면 많은 부분들을 개발할 수 있게 됨

케이스가 확정적인 것만 구현할 수 있다면 초급개발자, 케이스가 재귀적이면서 복합적인 상황들을 처리할 수 있다면 중급개발자

```javascript
const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] };
  const stack = [{ tag: result }];
  let curr,
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {}
  }
  return result;
};
```

함수를 디자인할 때는 인풋값과 리턴값을 디자인하는 것입니다

이 함수가 무엇을 하는지는 어떤 시그니처를 가지고 있느냐에 달려있습니다

parser 가 동작하는 방향은 children 을 어떻게 채우느냐에 달려있습니다

텍스트 덩어리는 구조적인 객체화를 시켜서 리턴하고 싶습니다

일차원 루프로는 순환적인 문제를 풀 수 없습니다

스택 수준을 오갈 수 있어야지만 일관된 루프로 처리할 수 있습니다

대상을 처리하는 것은 하나의 알고리즘이지만, 대상을 바꾸려면 스택을 오가야 하기 때문입니다

그래서 스택 구조가 바깥에 있고, 안에 알고리즘 구조가 있는 식으로 구현되어있습니다

현재 처리중인 스택을 curr 라고 부르기로 합니다

stack 은 전체 stack 루프를 위한 것입니다

tag 라는 것은 가상의 DOM 객체를 의미합니다

동적으로 루프가 변하는 루프를 짜봐야 합니다

안에 있는 이너루프는 j 값이 확정적입니다, 이 루프는 중간에 변하지 않습니다

그에 비해서 스택 루프는 루프회수가 얼마나 될지 결정되지 않았습니다

이너루프에서 스택에 변하면 루프회수가 늘어나거나 줄어듭니다, 따라서 동적 계획에 따른 루프입니다

계획되지 않은 루프는 위험하다고 생각하시면 안 됩니다

고급루프는 모두 런타임에서 루프회수가 변합니다

input 값에 따라 루프회수가 변합니다

동적 계획에 따른 루프 : 루프를 결정하는 요인이 이너루프를 돌면서 변할 수 있는 루프

중급 이상에서는 오히려 이런 루프가 기본입니다

루프를 돌 때, 무한루프가 될 수 있으니 안정적으로 확정루프가 될 수 있는지 확인하고 루프를 쓰라는 것은 모두 주니어들만을 위한 이야기입니다

실제로 루프회수가 얼마나 될지는 루프를 돌다가 깨닫는 경우가 많습니다

```javascript
const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] };
  const stack = [{ tag: result }];
  let curr,
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === "<") {
        // A, B의 경우
      } else {
        // C의 경우
      }
    }
  }
  return result;
};
```

처음부터 끝까지 쭉 훑는 것을 스캐너라고 부른다

이너루프는 받아온 문자열을 처음부터 끝까지 도는 것이니 스캐너이다

이너루프안에서 로직계산시에 i 를 건들일 수 있는데 i 는 루프를 결정하는 것이므로 위험하므로 i 를 cursor 에 대입함

<로 시작하면 태그로 볼 수 있다, 태그가 아니라면 C 즉 TEXT 타입이다

C 가 더 쉬우니 TEXT 처리부터 진행한다

```javascript
const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] };
  const stack = [{ tag: result }];
  let curr,
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === "<") {
        // A, B의 경우
      } else {
        const idx = input.indexOf("<", cursor);
        curr.tag.children.push({
          type: "text",
          text: input.substring(cursor, idx)
        });
        i = idx;
      }
    }
  }
  return result;
};
```

```html
<div>
  a
  <a>b</a>
  c
  <img/>
  d
</div>
```

<div> 뒤부터 <a> 앞까지는 모두 텍스트로 볼 수 있다

현재 바라보고 있는 스택에 있는 tag 의 자식에 텍스트 노드를 집어넣는다

그리고 그것을 처리해준 뒤 시점으로 i 를 이동시킨다

읽기에도 사용하고 쓰기에도 사용하면 혼란스러울 수 있으니
읽기 즉 조회 용도로 cursor 를 만들어서 알고리즘에서 사용하고,
실제로 인덱스를 옮겨야 하는 이벤트 시점에만 i 를 업데이트한다

코드가 별 것 아닐 때 역할을 인식해야 한다

이 역할은 독립적인가?

<를 찾기 직전까지를 묶어서 텍스트 노드로 만들어서 현재 스택의 자식으로 넣어준다 라는 알고리즘이 역할상 독립적입니다

그렇다면 즉시 함수화 시킨다

```javascript
const textNode = (input, cursor, curr) => {
  const idx = input.indexOf("<", cursor);
  curr.tag.children.push({
    type: "text",
    text: input.substring(cursor, idx)
  });
  return idx;
};
```

역할이 분리된 순간 즉시 함수로 간다, 코드 관리를 위해서

나중에 텍스트 노드에 더 많은 기능을 넣고 싶으면 함수만 수정하면 된다

단 cursor 나 curr 는 지역변수가 아니다, 그렇기에 인자로 받을 수 밖에 없다

코드를 그대로 보내고 모자란 것들을 인자로 만든다

바깥 쪽에 있는 프리미티브 변수를 갱신해야 할 때는 return 을 사용하여 바깥 쪽에서 갱신하도록 함

```javascript
const textNode = (input, cursor, curr) => {
  const idx = input.indexOf("<", cursor);
  curr.tag.children.push({
    type: "text",
    text: input.substring(cursor, idx)
  });
  return idx;
};

const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] };
  const stack = [{ tag: result }];
  let curr,
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === "<") {
        // A, B의 경우
      } else i = textNode(input, cursor, curr);
    }
  }
  return result;
};
```

역할을 인식한 순간 함수화를 하지 못하면 끝장이다, 겉잡을 수 없게 된다

이 시점에 분리해야지만 가능하다, 나중에 뜯어서 분리하려고 하면 새로 짜는 수 밖에 없다

이미 너무 많이 오염되어있을 것, 인식하자마자 바꿔야 한다

input 과 cursor 는 값에 의한 복사니까 문제없지만 curr 는 객체이니 나쁜 것은 아닌가?

나쁘다, 그러나 방지하려면 curr 를 받지 않고 idx 와 텍스트 노드 객체를 함께 담아 배열로 리턴하여 바깥 쪽에서 하나는 i 를, 하나는 curr 를 수정하는 식으로 구현할 수도 있다

코드는 응집성을 가지면서 결합도를 낮춰야 한다, 그 밸런스를 맞춰야 한다, 둘 모두를 추구할 수는 없다

여기서는 응집성을 추구한 것, 항상 선택의 기로에 서게 됩니다

만약 위처럼 curr 를 받지 않으면 응집성과 의존성 모두 낮아진다

항상 의사결정을 할 수 밖에 없습니다,

밸런스를 어떻게 지킬 것이냐는 상황에 따라 다르기 때문에 그 센스를 갖기가 어려워서 좋은 개발자가 되기 힘든 것이다

실무에서는 정답을 하나로 정할 수 없습니다, 항상 밸런스 문제가 있습니다

< 라는 weak 한 것에 의존하여 분기처리를 해도 되는가? HTML 도 그렇다

그래서 &lt;으로 이스케이프하지 않으면 <를 발동 트리거로 쓸 수 없다

닫는 태그(&gt;)는 발동 트리거가 아니기 때문에 무관하다

이런 parser 들을 구현하다보면 separator 혹은 token 이라고 불리는 문자열들이 만들어지기 마련이다

얘네들은 값으로 쓰고 싶다면 이스케이프 처리가 필요하다

중요한 요령은 무조건 쉬운 것부터 처리한다는 것

왜 그런가? 쉬운 코드는 의존성이 낮고 독립된 기능일 가능성이 높습니다

쉬운 것부터 구현해야 그것을 의존하는 다른 코드를 짜기가 수월합니다

복잡한 것부터 짜면 자기 것인줄 알았던 부분이 공유되어야 하는 부분도 크고,

남에게 의존할 것이 있었는데 자기가 처리하게 되면서 중복도 생겨나곤 합니다

되도록이면 쉬운 것부터 짜야 합니다

그래야지 더 견고하고 의존성이 낮은 모듈로부터 의존성 높은 모듈을 짜나갈 수 있습니다

항상 쉬운 것부터 짜세요

< 로 발동되는 케이스는 열린 태그, 닫힌 태그, self 종료 태그(<img/>) 세 가지로 나눌 수 있습니다

```javascript
const textNode = (input, cursor, curr) => {...};

const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] };
  const stack = [{ tag: result }];
  let curr,
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === "<") {
        const idx = input.indexOf('>', cursor);
        i = idx + 1;
      } else i = textNode(input, cursor, curr);
    }
  }
  return result;
};
```

indexOf 의 두 번째 인자를 주지 않으면 우리가 의도한 곳을 찾지 못할 것입니다

세 가지 케이스 중에 무엇인지와 무관하게 그 다음 번 i 가 무엇인지는 확정지을 수 있습니다

A 와 B 케이스의 공통점은 <로 시작해서 >로 끝난다는 것, 즉 태그이다

코드를 잘 짜면 코드를 읽을 수 있습니다

케이스의 공통점을 찾아서 먼저 처리해준다, 코드가 중복되지 않도록

이렇게 바라보지 않으면 똑같은 행동을 세 번 처리하게 되고 나중에 중복을 제거하기가 어려워진다

그럴 것이면 미리 눈을 훈련해서 공통점을 추출할 수 있도록 하는 수 밖에 없습니다

가볍게 보지 말고, 함부로 보지 말고, 더 깊게 바라보세요

얘네들은 분명 친척일거야 라는 관점으로-

똑같은 애들부터 찾아서 처리해야 합니다

남들이 짠 코드에도 많은 고민과 의도가 숨겨져있을 것이라 생각하고 바라보셔야 합니다

오픈소스나 프로개발자들이 짠 코드에는 굉장히 많은 메타포가 숨어있습니다

언어 사용에 익숙한 사람들에게는 그것이 읽힙니다

한국어로 다양하게 노란색을 표현해도 뉘앙스를 느끼시듯이 언어에 익숙해지면 코드를 보고 뉘앙스를 느낍니다 똑같이

그러려면 여러분들이 뉘앙스를 표현할만큼 언어를 깊이 써야 합니다

코드에 다 의도가 있습니다

<div> 와 <img/>는 태그를 연다는 공통점이 있습니다, </a>는 외톨이

태그를 열면 어떤 일이 일어나야 하는가? 태그가 닫힐 때까지의 그 이후 내용들은 모두 해당 태그의 자식이 되어야 함

물론 <img/>는 열리자마자 닫혀서 그 이후 태그들에게 내용들이 향해야 곳에 주지 않지만, 열어서 새로운 태그를 만들어냈다는 점은 동일

사물을 보고 데이터 애널리시스를 하는데 추상화된 공통점과 재귀적인 로직을 발견하는 것이 우리의 몫입니다

그것을 못하면 코드는 계속 길어지고 if 로 분기만 하게 됩니다

단위테스트를 만들거나 TDD 를 해도 나쁜 개발자는 나쁜 데이터 애널리시스를 하고 나쁜 로직을 짰기 때문에

테스트 케이스를 100 개 만들면 100 개의 케이스가 통과한 것만 증명하지, 101 번째 케이스에 문제가 없다는 것을 증명하지 못합니다

원래 코드가 나쁘거나 설계가 잘못 되었으면 TDD 를 해도 소용이 없다

TDD 를 도입하는 것과 데이터를 바라보거나 추상적인 로직을 이해하는 것은 별개의 문제이다

어차피 데이터 애널리시스를 잘못하면 무엇을 해도 나쁜 코드가 나온다

좋은 코드를 짜는 비밀은 TDD 에 있는 것이 아니라 데이터를 이해하고 재귀적인 로직을 찾아내거나 추상화된 공통점을 찾아낼 수 있는지 역할을 이해하는지 여부에 달려있다

```javascript
const textNode = (input, cursor, curr) => {...};

const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] };
  const stack = [{ tag: result }];
  let curr,
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === "<") {
        const idx = input.indexOf('>', cursor);
        i = idx + 1;
        if (input[cursor + 1] === '/') {}
        else {
          if (input[idx - 1] === '/') {}
          else {}
        }
      } else i = textNode(input, cursor, curr);
    }
  }
  return result;
};
```

`input[cursor + 1] === '/'`는 </a> 케이스

그 뒤에 `input[idx - 1] === '/'` 여부로 <img/> (true)와 <div> (false)를 나눈다

멘탈 모델이 그려지면, 코드로도 똑같이 표현되는 것이 정상이에요, 그래야 유지보수를 할 수 있습니다

그렇기 때문에 주석이 불필요합니다

그럼에도 주석이 필요하다고 느낀다면 개발자를 그만 둘 때입니다

여는 태그는 닫는 태그를 알아야 하고, 닫는 태그는 여는 태그를 알아야 하니, <img/>부터 구현하도록 합니다

현실 세계를 인식해서 그 현실 세계를 해결할 코드를 짠다면 반대로 현실 세계에 대입해도 코드가 정확히 매핑되어야 합니다

바른 데이터 애널리시스를 했다면 코드는 거의 매핑이다, 어렵지 않습니다

여러분들이 알고리즘을 짜는데 어려움을 겪거나 애플리케이션을 못 만들어내겠다면 데이터 애널리시스에 실패한 것입니다

데이터 애널리시스를 잘 했으면 바로 매핑됩니다

```javascript
const textNode = (input, cursor, curr) => {...};

const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] };
  const stack = [{ tag: result }];
  let curr,
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === "<") {
        const idx = input.indexOf('>', cursor);
        i = idx + 1;
        if (input[cursor + 1] === '/') {}
        else {
          let name, isClose;
          if (input[idx - 1] === '/') {
            name = input.substring(cursor + 1, idx - 1), isClose = true;
          } else {
            name = input.substring(cursor + 1, idx), isClose = false;
          }
        }
      } else i = textNode(input, cursor, curr);
    }
  }
  return result;
};
```

대응되도록 똑같은 쌍을 만들어줍니다, 느껴지는 차이를 그대로 코드로 옮겨온 것입니다, 한국어를 코드로 번역하는 과정

정확하게 한국어로 상황을 인식할 수 있다면 코드는 그냥 번역하면 됩니다

번역할 때 뉘앙스를 줘라, if else 라는 두 가지 이지선다를 주고 이지선다가 똑같이 동형으로 똑같은 리듬이 느껴지는 것을 원했습니다

조건에 따라 약간의 차이가 존재한다는 뉘앙스가 전달되어야 합니다

문학적인 표현이 필요한게 아니라, 데이터 애널리시스에 대한 관점을 정확하게 표현하는 것은 유지보수를 위해 항상 필요합니다

섬세하다고 느껴지시면 맞습니다, 고급 개발자들은 언어에 익숙하기 때문에 섬세하게 짭니다

```javascript
const idx = input.indexOf(">", cursor);
i = idx + 1;
if (input[cursor + 1] === "/") {
} else {
  let name, isClose;
  if (input[idx - 1] === "/") {
    (name = input.substring(cursor + 1, idx - 1)), (isClose = true);
  } else {
    (name = input.substring(cursor + 1, idx)), (isClose = false);
  }
  const tag = { name, type: "node", children: [] };
  curr.tag.children.push(tag);
  if (!isClose) {
    stack.push({ tag, back: curr });
    break;
  }
}
```

차이점을 처리했다면 더이상 차이를 케이스로 인식하지 않아도 됩니다

원래 케이스였던 것을 값으로 흡수한 뒤에는 더이상 케이스를 인식하지 않고 값을 사용하는 일반화된 알고리즘이 나옵니다

메모리와 연산은 교환됩니다

차이를 일으키는 연산을 메모리로 흡수한 다음에, 메모리를 가리키는 하나의 연산만 기술하면 됩니다

name 을 화이트리스트라고 부릅니다, 함수의 인자를 필터링해서 안정적인 조건을 만든다든지 서버에서 내려준 JSON 을 View 가 소비하기 좋게 바꾼다는지 등

알고리즘은 그러한 복잡성을 모두 제거하고 복잡성이 모두 제거되어 안정화된 상태에서 화이트리스트를 가지고 알고리즘을 하나만 구현하는 것이 제일 최선이다

알고리즘으로 함부로 돌입하지 말고, 화이트리스트 작성에 굉장히 공을 많이 들인 다음에 완전히 정제되어있는 화이트리스트용 알고리즘만 하나 구현하시는 것이 최선입니다, 가장 유지보수도 수월합니다

번역층을 거치면 화이트리스트를 기반으로 이 알고리즘은 잘 동작할 것이라고 보장됩니다

stack 에 push 후 break 를 하는 것은 마치 함수를 호출할 때 리턴포인트를 알고 끝나면 그곳으로 돌아가는 것의 수동 버전과 같습니다

```javascript
const elementNode = (input, cursor, idx, curr, stack) => {
  let name, isClose;
  if (input[idx - 1] === '/') {
    name = input.substring(cursor + 1, idx - 1), isClose = true;
  } else {
    name = input.substring(cursor + 1, idx), isClose = false;
  }
  const tag = {name, type: 'node', children: []};
  curr.tag.children.push(tag);
  if (!isClose) {
    stack.push({ tag, back: curr });
    return true;
  }
  return false;
}

const textNode = (input, cursor, curr) => {...};
const parser = input => {
  input = input.trim();
  const result = { name: 'ROOT', type: 'node', children: [] }, stack = [];
  let curr = { tag: result }, i = 0, j = input.length;
  while (curr = stack.pop()) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === '<') {
        const idx = input.indexOf('>', cursor);
        i = idx + 1;
        if (input[cursor + 1] === '/') {
          curr = curr.back;
        } else {
          if (elementNode(input, cursor, idx, curr, stack)) break;
        }
      } else i = textNode(input, cursor, curr);
    }
  }
}
```

break 는 외부 제어 통제이므로 elementNode 함수 안에서 통제할 수 없으므로 flag 변수로 리턴해줍니다

코드의 가독성은 어떻게 확보하는가?

변수명, 코드컨벤션 등으로 가독성이 확보되지 않습니다

알고리즘, 수학적 함수, 연산 등은 어렵습니다

컴퓨터가 연산해야 할 것을 머리속으로 생각하면서 읽어야 하기 때문입니다

어떤 코드가 읽기 쉬운가? 역할에게 위임하는 코드

역할을 인식해서 역할에게 위임하는 코드를 짜야 합니다, 오직 그것만이 쉽게 합니다

알고리즘 등은 원래 무조건 어렵습니다

적절한 역할모델로 위임되어서 그것들간의 통신과 협업만 볼 수 있는 코드가 가독성 높은 코드입니다

console.log(parser(`<div>a<a>b</a>c<img/>d</div>`));

HTML 에서 태그간 엔터만 쳐도 텍스트노드가 생성됩니다

HTML 압축은 DOM 의 렌더링 대상을 줄여줄 수 있게 됩니다

CSS, JS 압축보다 HTML 압축을 하면 생성되는 노드수를 줄일 수 있다, 브라우저 부하를 줄일 수 있습니다

```javascript
const elementNode = (input, cursor, idx, curr, stack) => {
  const isClose = input[idx - 1] === "/";
  const tag = {
    name: input.substring(cursor + 1, idx - (isClose ? 1 : 0)),
    type: "node",
    children: []
  };
  curr.tag.children.push(tag);
  if (!isClose) {
    stack.push({ tag, back: curr });
    return true;
  }
  return false;
};
```

중복된 코드를 줄이는 수준은 우리의 수준에 달려있어요

켄트벡이 말하길, 중복은 제거하는게 아니라 발견하는 것이라 했습니다

프로그래머의 수준에 따라서 코드 수준의 중복, 아키텍처 수준의 중복, 데이터 상의 중복 등을 깨닫게 됩니다

궁극적으로 개발 중복을 다 인식하게 되면, 개발을 모두 직접 짜는 행위가 없어지고 솔루션화 됩니다

처음에는 케이스 바이 케이스로 코드로 만들다가 공통되는 부분들이 자동으로 처리되다가 나중에는 코드로 구현할 것이 하나도 없이 명령만으로 다 되게끔 바뀝니다

코드를 이따금씩 바라보면서 중복이 보일 때마다 내 실력이 올라갔구나 라고 느끼시면 됩니다

코드 수준의 중복을 바라보는 것과 아키텍처 수준의 중복을 발견해내는 것이 동시에 올라가는 것이 아니라 따로 따로 훈련해야 올라가게 되어있습니다

코드 수준의 중복에 대한 이해는 언어에 대한 바른 이해와 문법적인 해박한 사용방법에 대해 이해할수록 높일 수 있습니다

의미를 훼손하지 않고 줄였음에도 초보자들에게는 암호처럼 보일 수 있습니다

왜냐하면 초보자들은 언어를 네이티브처럼 쓰지 않기 때문입니다

아키텍처 레벨은 역할 관계를 인식하고, 책임이 어디까지 닿아있는지, 얼마나 확장가능성이 있는지 등을 보는 눈에서부터 어디가 중복이고 어디를 레이어로 나눌지에 대한 눈이 생겨야 합니다

데이터 중복은 전통적인 RDB 에 있는 정규화를 비롯해서 다양한 데이터에 대한 중복을 제거하는 안정적인 로직들이 많이 있습니다

제가 코드를 인정하는 기준은 딱 하나에요, 컴퓨터가 인정하면 나도 인정

여러분들이 남의 코드를 바라보면서 스트레스를 받는 이유는, 자꾸 비이성적인 것들이 프레임처럼 작용해서 가로막고 있어서 코드를 못 읽게 만들기 때문입니다

심리적인 장벽이 힘들게 만듭니다

남의 코드를 바라보면서 내가 원하는 스타일이 아니면 힘들게 여기는 것입니다

심리적인 장벽을 없애고 선입견을 내려놓고 바라보면 그냥 보입니다

사람들은 자기 코드를 인정해주고 잘 돌아가지 않는 부분에 대해서 조언을 해주거나 더 좋은 코드가 되기 위한 알고리즘을 제안해주는 사람을 따르기 마련입니다

여러분들이 선입견을 버리지 않으면 그렇지 못한 사람이 될 것입니다

코드는 잘 동작하고 바른 로직으로 되어있으면 다 바른 코드입니다

스타일은 별로 중요하지 않습니다

```javascript
const parser = input => {
  input = input.trim();
  const result = { name: "ROOT", type: "node", children: [] },
    stack = [];
  let curr = { tag: result },
    i = 0,
    j = input.length;
  while ((curr = stack.pop())) {
    while (i < j) {
      const cursor = i;
      if (input[cursor] === "<") {
        const idx = input.indexOf(">", cursor);
        i = idx + 1;
        if (input[cursor + 1] === "/") {
          curr = curr.back;
        } else {
          if (elementNode(input, cursor, idx, curr, stack)) break;
        }
      } else i = textNode(input, cursor, curr);
    }
  }
};
```

HTML 은 멀티 스택 상황이 발생하지 않습니다, 컨텍스트가 동시에 두 개가 진행되지 않습니다, 이런 경우에는 스택 배열이 필요없고 하나의 변수에서만 교체해주면 됩니다, 하지만 많은 경우에는 여러 개를 동시에 유지하면서 따로 따로 주기를 가지고 컨텍스트가 교체되곤 합니다, 지금은 curr 하나만 가지고 있으면 됩니다

코드를 만드는데 차이는 디테일에서 생겨납니다, 오늘까지 해서 함수를 배우신 것입니다, 게다가 함수형 프로그래밍은 다루지 않았고, 루틴을 다루는 방법만 배운 것입니다, 루틴에 먼저 익숙해지는 것이야말로 중급개발자로 가는 기본이고, 루틴에 익숙하지 않은 사람은 객체구조를 만들어도 어차피 엉망으로 만들게 되어있습니다

Q0. 위 구조를 꼬리물기 최적화되는 재귀로 바꿀 수는 없는가? (위를 2 단 루프로 구성한 것은 stack point 를 인식해서 재귀함수로 만들기 더 쉬우시라는 의미였다, 이너루프만 함수로 만들면 되기 때문에)

Q1. stack 을 벗어날 때는 바깥 쪽 stack 으로 나가지 않으면서 왜 stack 을 추가할 때는 바깥 쪽 stack 으로 나갔다가 들어오지? stack 구조를 모두 지우고 curr 를 교체하는 것만으로 구현할 수 있지 않은가?

Q2. JSON 문자열을 받아 JSON Object 를 리턴해주는 JSON Parser, HTML 에서의 C 타입에 해당하는 것이 string, integer, boolean, null 인 것, 그리고 object 시작하면 object 닫아야 하고, array 시작했으면 array 닫아야 함, " 안에 중괄호나 대괄호가 있으면 그저 문자입니다, HTML 와 달리 이스케이프를 지원해주어야 합니다

Q3. 속성이 포함된 html 을 파싱하기 `<div style="background:red" class="test">...</div>` 이런 식으로 태그 안의 여러 속성을 각 노드에서 attribute 라는 별도의 배열에 저장하기
