## 3. 컬렉션 중심 프로그래밍 (1) - reduce, 제너레이터, 다형성

reduce method가 이미 있는데, 왜 새로 만들어야 하는가?

=> Array가 아닌 모든 유사배열에는 해당 reduce를 적용할 수 없으므로 함수형에 걸맞는 reduce를 만든다

```javascript
const reduce = (f, coll, acc) => {
  const iter = coll[Symbol.iterator]();
  if (acc === undefined) acc = iter.next().value;

  for (const v of iter) {
    acc = f(acc, v);
  }

  return acc;
}

// console.log(reduce((acc, a) => acc + a, new Set([1, 2, 3]), 0)) == 6
// console.log(reduce((acc, a) => acc + a, new Set([1, 2, 3]))) == 6
```

Symbol.iterator가 구현된 모든 타입에 쓰일 수 있는 reduce

```javascript
const collIter = coll => coll[Symbol.iterator]();

const reduce = (f, coll, acc) => {
  const iter = collIter(coll);
  if (acc === undefined) acc = iter.next().value;

  for (const v of iter) {
    acc = f(acc, v);
  }
    
  return acc;
}
```

collIter 수정을 통해 다형성을 높일 수 있게 됨, 선언형과 명령형의 큰 차이점

위 장점이 가능해지려면 인자와 리턴값으로 소통해야 하고, 각각의 형을 철저하게 지켜야 함, 이 전제가 지켜진다면 함수 내부 로직이 변경되더라도 해당 함수만 테스트하면 전체 시스템이 유지됨

객체지향은 어떤 클래스와 연결되어있느냐에 따라 아웃풋의 형도 다양하게 하려고 한다, 그렇게 된다면 조건문 등이 한 번 잘못 수정되면 전체 시스템이 망가질 수도 있음

```javascript
function *valuesIter(obj) {
  for (const k in obj) yield obj[k];
}
    
const toArray = obj => [...valuesIter(obj)];
    
console.log(
  reduce((acc, a) => acc + a, valuesIter({ "a": 1, "b": 2, "c": 3 }), 0)
)
```

for문 안에서 이루어지는 로직이 늘어나는 것이 아니라면, 성능상 이슈는 없을 것, for문 안에서 조건문이 있는 것도 별 다른 영향을 미치지 않는다

퍼포먼스 확인 필요시 console.time, console.timeEnd를 사용해보세요

valuesIter를 사용하면 새로운 배열을 만들어내는 것이 아니라 내장 프로토콜을 사용하여 기존 값을 그대로 사용하는 것이므로 성능상 아무런 문제가 없다 (valuesIter 와 toArray는 성능상 차이가 많이 발생한다)

Array의 [Symbol.iterator]\(\)는 values로 구현되어있는데 Map의 [Symbol.iterator]\(\)는 entries로 구현되어있다

각각이 어떻게 구현되어있구나 에서만 그치면 안 되고, 어떤 iterator를 리턴하는 함수를 만들 때, 다양하게 구현할 수 있다고 생각하는게 필요하다

```javascript
function *entriesIter(obj) {
  for (const k in obj) yield [k, obj[k]];
}

console.log(
  reduce((acc, [k, v]) => acc + k + v, entriesIter({ a: 1, b: 2, c: 3 }), 0)
)
```

함수형 프로그래밍은 평가 순서가 중요하지 않은게 대단히 중요한 컨셉이므로 key라는 개념이 없이 map이나 reduce를 만들 수 있어야 함, 하지만 key가 필요하다면 entriesIter를 사용해야 한다

```javascript
const collIter = coll =>
  coll.constructor == Object
    ? valuesIter(coll)
    : coll[Symbol.iterator]();
```

collIter를 확장해보자, 기존 collIter는 array, array-like, iterator를 지원했으나, key&value 객체도 지원하도록 확장함

JS는 모든 것이 key&value 쌍이므로 우리가 지원하려는 범위를 설정해야 함

하지만 경험상 key&value 쌍으로 사용하는 것은 데이터를 다루기 위해서 사용하지, 각각의 key&value간 동질성이 없을 경우 컬렉션으로 취급하여 loop를 돌 일이 존재하지 않을 것, 그렇기 때문에 JS의 모든 객체가 아니라 컬렉션으로서 의미가 있는 `{a: 1, b: 2}` 와 같은 형태만을 key&value 쌍의 컬렉션으로 취급하자

구분하는 방법은 `({}).constructor == Object`이다

instanceof는 상속 구조를 전부 쫓아가서 확인하는 것이고, constructor는 해당 constructor로 만들어진 것이 맞는지를 확인함 (constructor가 보다 안전하다) `coll.constructor == Object`가 plain object인지를 확인할 수 있는 구문

---

## 3. 컬렉션 중심 프로그래밍 (2) - reduce 활용 (posts, users), countBy, groupBy

```javascript
function *reverseIter(list) {
  const l = list.length;
  while (l--) yield list[l];
}
    
console.log(
  reduce((a, b) => a - b, reverseIter([1, 2, 3, 4]))
)
```

함수 통과를 통해 내가 원하는 로직을 구현



reduce를 사용하는 목적 : collection을 처음부터 끝까지 순회하며 무엇인가를 만들고 싶을 때



reduce에서 initValue를 인자로 받지 않았을 때의 전제조건 : reduce의 보조함수의 인자들(acc, a)이 서로 같은 형임을 보장하고 따라서 reduce 전체의 결과값도 보조함수의 인자들의 형과 같다



reduce에서 initValue를 인자로 받았을 때의 전제 : 보조함수의 인자들간의 형이 다를 수 있다



map은 주어진 자료구조와 동일한 형인데 데이터가 바뀐 것을 만들 때, reduce는 보통 형이 다른 것을 만들 때



reduce, map, filter, find 가 가장 중요하다



find는 어떤 지점을 찾을 때, 중간에 멈추기 위해서 사용한다



reduce나 find로 만들어진 함수들도 해당 뼈대의 역할을 할 것



```javascript
const posts = [
  { id: 1, body: '내용1', comments: [{}, {}] },
  { id: 2, body: '내용2', comments: [{}] },
  { id: 3, body: '내용3', comments: [{}, {}, {}] },
  { id: 4, body: '내용4', comments: [{}, {}, {}] },
];

// posts의 comments 총 개수
reduce((acc, ({ comments })) => acc + comments.length, posts, 0)
```

구하고자 하는 형이 컬렉션의 데이터와 다르다 => 기본값을 주어야 한다



내가 인풋할 posts과 보조함수에서 맞춰줘야 할 규격을 알 수 있으므로 돌릴 수 있는 모든 형을 접을 수 있다



보조함수를 통해 컬렉션 안에서 원하는 값을 잘 얻어낼 수 있다면 다형성을 점차 높일 수 있다

```javascript
const users = [
  { id: 1, name: 'name1', age: 30 },
  { id: 2, name: 'name2', age: 31 },
  { id: 3, name: 'name3', age: 32 },
  { id: 4, name: 'name4', age: 31 },
  { id: 5, name: 'name5', age: 32 },
]

// 나이를 키로 하고, 값은 자기 자신이 담겨지도록 해주세요
// 값으로 개수가 되도록도 해주세요

const res1 = reduce((acc, v) =>
  (acc[v.age] ? acc[v.age].push(v) : acc[v.age] = [v], acc)
, users, {})

const res1i = reduce((group, u) => {
  (group[u.age] || (group[u.age] = [])).push(u);
  return group;
  }, users, {})
)

const res2 = reduce((acc, { age }) =>
  (acc[age] ? acc[age] += 1 : acc[age] = 1, acc)
, users, {})

const res2i = reduce((counts, u) => {
  counts[u.age] ? counts[u.age]++ : counts[u.age] = 1;
  return counts;
  }, users, {})
)

const incSel = (parent, k) =>
  (parent[k] ? parent[k]++ : parent[k] = 1, parent)

incSel(incSel(incSel({}, 'a'), 'a'), 'b')
// {a: 2, b: 1}
// 위와 같이 연속적으로 재귀를 실행해주는 것이 reduce의 역할일 것
// 설계해야 하는 부분은 인자에 무엇이 넘어가느냐? 돌려주는 것은 reduce의 역할

const res2i2 = reduce((counts, u) =>
  incSel(counts, u.age)
}, users, {});
```

```javascript
const countBy = (f, coll) =>
  reduce((counts, a) => incSel(counts, f(a)), coll, {});

countBy(u => u.age, users)

const pushSel = (parent, k, v) =>
  ((parent[k] || (parent[k] = [])).push(v), arr)

const res1i2 = reduce((group, u) => pushSel(group, u.age, u), users, {})

const groupBy = (f, coll) => reduce((group, a) => pushSel(group, f(a), a), coll, {});

groupBy(u => u.age, users);

countBy(a => a, [1, 2, 3, 3, 4, 5, 5, 10, 10, 10]);
// {1: 1, 2: 1, 3: 2, 4: 1, 5: 2, 10: 3}

countBy(a => a + 10, [1, 2, 3, 3, 4, 5, 5, 10, 10, 10]);
// {11: 1, 12: 1, 13: 2, 14: 1, 15: 2, 20: 3}
// 리턴으로 나오는 값과 동일한 수들의 개수를 세는 것
// a로부터 접근하는 것도 가능하고, a의 메서드를 실행하는 것도 가능하고, a를 가지고 연산을 하는 것도 가능하다

groupBy(a => a, [1, 2, 3, 3, 4, 5, 5, 10, 10, 10]);
// {1: [1], 2: [2], 3: [3, 3], 4: [4], 5: [5, 5], 10: [10, 10, 10]}
```

```javascript
// Haskel에서 유명한 함수인 까닭은, 뭔가 감싸져서 Monad를 통해서 안에 있는 값을 꺼내서 쓰는 식으로 프로그래밍하기 때문
const identity = a => a;

countBy(identity, [1, 2, 3, 3, 4, 5, 5, 10, 10, 10]);
groupBy(identity, [1, 2, 3, 3, 4, 5, 5, 10, 10, 10]);

const count = coll => countBy(identity, coll);

count([1, 2, 3, 3, 4, 5, 5, 10, 10, 10]);
```

---

## 3. 컬렉션 중심 프로그래밍 (3) - Promise, then, 동기/비동기 다형성, reduce에 Promise 다형성 추가

Promise를 잘 다루느냐가 정말 중요합니다, 데이터 뿐만 아니라 UI를 다룰 때도 정말 잘 쓰입니다, 컨펌창을 띄우고 코드를 멈추도록 할 수도 있고, 친구목록을 띄우는 것도 할 수 있어요, 정말 중요해요

유려하고 성능적으로 훌륭한 앱을 만들 때도 중요해요, UI에서는 보통 두 가지 일을 동시에 이루어지는 일이 빈번하게 발생해요, 창이 올라오면서 API 요청하고 회신오면 그린다든지, 제대로 제어하지 못하면 UI가 안 좋게 나와요, Promise를 쓰지 않으면 상황별 재귀를 사용해야만 해요, Promise는 비동기 상황을 다루는 값이에요, Promise를 잘 다루는 함수를 만들어놓고 그것을 잘 쓰는 것도 중요합니다

모든 코드를 짤 때 항상 동기와 비동기가 혼재될 수 있다고 가정하겠습니다, 난이도가 확 증가할 것입니다

```javascript
// 이것이거나 그것이 아니거나 로 나누어서 생각하시면 좀 더 쉬워집니다, 문제를 나누어서 생각해야 합니다
// Promise.resolve(10)에 접근하려면 then으로 꺼내야 합니다
// Promise에서 then으로 꺼낸 값이 collection이라고 가정하는 reduce를 만들어봅시다

async function reduce(f, coll, acc) {
  const iter = collIter(await coll);
  acc = acc === undefined ? iter.next().value : acc;
  for (const v of iter) {
    acc = f(acc, v);
  }
}

(async function() {
  console.log(
    reduce((a, b) => a + b, Promise.resolve([1, 2, 3]))
  )
})();

// async, await이 callback 지옥을 해결했다는 것은 허구입니다, 위처럼 해결하면 계속 위처럼 해결해야 합니다
// 또한 아래 동작도 안 되기 시작합니다

console.log(
  reduce((a, b) => a + b, [1, 2, 3])
)

// async 키워드가 있으면 Promise를 리턴하기 때문입니다
// 그런데 비동기가 불필요하게 일어나면 성능적으로 대단히 불리합니다
// 비동기를 처리하기 위해 모든 것을 비동기화해서 처리하게 되면 성능적으로 굉장히 불리하게 됩니다
// 동기적으로 처리할 수 없으면 항상 async await을 써야 하고, 루프마다 비동기가 일어나기 때문에 굉장히 느려집니다
// 브라우저에서 렌더링되는 타이밍은 콜스택이 비워질 때인데, 중간에 비동기가 계속 일어나면 렌더링이 중간중간 이루어지면서 원하는 UI가 안 나올 가능성이 높습니다
// 제일 좋은 해결책은 async, await보다는 함수형적으로 동기와 비동기를 다형성으로 잘 관리하는 것입니다

// acc가 Promise냐 아니냐에 따라 달리 동작해야 합니다
const then1 = f => a => a instanceof Promise ? a.then(f) : f(a);

then(console.log)(10); // 10
then(console.log)(Promise.resolve(10)); // 10

const then2 = (f, a) => a instanceof Promise ? a.then(f) : f(a);

function reduce(f, coll, acc) {
  return then2(function(coll) {
    const iter = collIter(coll);
    acc = acc === undefined ? iter.next().value : acc;
    for (const v of iter) {
      acc = f(acc, v);
    }
    return acc;
  }, coll);
}

console.log(reduce((a, b) => a + b, [1, 2, 3, 4]));
reduce((a, b) => a + b, Promise.resolve([1, 2, 3, 4])).then(console.log);
reduce((a, b) => Promise.resolve(a + b), [1, 2, 3, 4]).then(console.log);

// 똑같은 일을 또 해야 해? 재귀를 떠올리세요
// 0. 재귀를 돌리고자 하는 구간을 일단 함수로 싸세요, 그 다음에 고민하세요
// 바꿔나가는 중간 중간에 무너뜨리지 않으면서 짜야 합니다, 중요합니다
// 1. 

function reduce(f, coll, acc) {
  return then2(function(coll) {
    const iter = collIter(coll);
    acc = acc === undefined ? iter.next().value : acc;
      
    return function recur(acc) {
      for (const v of iter) {
        acc = f(acc, v);
        if (acc instanceof Promise) {
          return acc.then(recur);
        }
      }
      return acc;
    }(acc);
  }, coll);
}

function reduce(f, coll, acc) {
  return then2(function(coll) {
    const iter = collIter(coll);
    acc = acc === undefined ? iter.next().value : acc;

    return then2(function recur(acc) {
      for (const v of iter)
        if ((acc = f(acc, v)) instanceof Promise) return acc.then(recur);
      return acc;
    }, acc);
  }, coll);
}

// async, await은 동시에 실행시키고 싶은 경우 모두 순차적으로 실행되어서 원하는 동작이 제대로 안 될 수도 있습니다
// 명령행으로도 함수형으로도 동기와 비동기를 동시에 다룰줄 알아야 합니다
```

