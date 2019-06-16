layout: post
title: "React Redux Hooks"
categories: Dev
tags: Dev
date: 2019-06-16

---

- https://velog.io/@velopert/react-hooks
- https://velog.io/@velopert/react-redux-hooks
- https://react-redux.js.org/next/api/hooks



드디어 react-redux 7.1.0이 업데이트되었고, 그로부터 얼마 뒤에 @types/react-redux도 업데이트되어 프로젝트에서 hooks를 통해 redux를 사용할 수 있게 되었다 (connect 안녕!)



Velopert님의 포스팅을 읽고 기본적인 사용 방법을 익혔다면, 공식문서를 통해 주의사항이나 최적화방법 등을 익혀보자



주로 사용할 API는 `useSelector`, `useDispatch`일 것이다



hooks 특성상 class 때와는 다른 방식의 최적화라고 느꼈다

class형 component는 데이터의 변화에 따라 render 또는 life cycle 관련 함수가 재호출되었지만, hooks는 데이터가 변화할 때마다 함수 전체가 재호출된다

그래서 공식문서에서 소개하는 최적화 방안들도 (호출 자체를 줄일 수 없으니) 호출했을 때 이전 호출의 결과를 바로 리턴해줄 수 있도록 판단하는 조건을 잘 갖춰두도록 하는 것에 초점이 맞춰져있다고 느꼈다



useSelector의 경우 mapStateToProps와 유사하지만 몇 가지 다른 지점이 있다

객체가 아닌 어떤 값이든 리턴할 수 있고, props를 직접적으로 인수로 넘겨받지 않는다

가장 중요한 차이점은 기존 값과의 비교를 할 때 기본적으로 ===를 사용하므로 객체로 리턴받으면 값이 동일하더라도 디스패치시마다 항상 새로 렌더링하게 된다

(문서에 따르면 mapStateToProps는 property별로 순회하며 값을 확인해서 리렌더링 여부를 판단해주었다고 한다)

따라서 선택지는 두 가지인데, 객체를 반환받지 않고, 각각의 값별로 useSelector를 쓰거나 shallowEqual를 export해서 두 번째 인자로 넣어주는 것이다


hooks에서 함수를 캐싱해둘 수 있는 방법으로는 useCallback이 있다

실제 내부 구현을 어떻게 했는지는 모르지만, 클로저로 참조하고 있는 변수에 대해서는 deps에 지정하지 않고도 기대한 결과가 나올 것이라 생각했다

그러나 그렇지 않아서 사실상 함수에서 사용하는 모든 변수를 두 번째 인자인 deps에 언급해주어야 한다고 느꼈다

기존 connect 방식에서는 actionCreator를 import하고, mapDispatchToProps에 넣어주고, props에 있는 actionCreator명을 호출해주어야 하는 번거로움이 있었다

이제는 import해오고 useDispatch를 통해 받아온 dispatch에 넘겨주기만 하면 되어서 세상 편하다
