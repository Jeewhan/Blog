layout: post
title: "리듀서 초기화"
categories: Dev
tags: Dev
date: 2019-07-28

---

리덕스는 UI -> Action Creator -> Action -> Store -> Reducer -> UI 라는 흐름을 따른다



그런데 매번 하나의 흐름을 만들어낼 때마다 위 과정에 해당하는 것들을 하나씩 더 만들어주기 마련이고, 그 흐름은 대체로 애플리케이션 전체에서 1~2곳 정도 쓰이는 경우가 많다



그래서 Ducks 패턴 등이 나왔다고 생각한다



Ducks 패턴은 첫 문장에서 설명한 흐름을 여러 파일에 나누어 각 단계별로 모은 파일로 관리하는 것이 아니라, 도메인(?)을 기준으로 파일을 생성하여 관리하는 방식으로 응집성(?)을 높이고자 했다고 이해했다



위와 비슷하게 모든 리듀서와 관련해서 반복되는 상황을 찾아 해결하려고 했던 나름의 시도를 적어보고자 한다



여러 리듀서들에서 모두 사용할법한 액션은 무엇일까? 바로 초기화이다



지금의 프로젝트를 맡게 되었을 때 하나의 액션 크리에이터 내에서 여러 번의 dispatch가 이루어지고, 그것이 곧 여러 차례의 리렌더로 이어지는 것을 보고 해결하고자 방법을 찾고 있을 때, 지인 분께서 어차피 액션은 모든 리듀서에 전파되기에 서로 다른 리듀서를 업데이트하고 싶다면 그것을 하나의 액션으로 묶으면 되지 않겠냐는 말씀을 하셨다



생각하지 못 했던 방법이었고, 실제로 실험해보았을 때도 기대대로 잘 동작하였다



머릿속에서 추상적으로 이해하였을 때는 하나의 액션이 하나의 리듀서에만 특정지어져야 할 것 같았지만 실제로는 그렇지 않았다



위 조언에서 힌트를 얻어서 만든 액션 크리에이터가 아래와 같다

```typescript
export const initialize = (payload: object) => ({ type: INITIALIZE, payload });
```



위 액션이 여러 리듀서에 존재한다면 각 리듀서에서는 자신과 관련된 액션인지를 어떻게 구분할 것인가?

그 코드는 아래와 같다



```typescript
const initialState: IEngagementState = {
  bookmarks: [],
  carts: [],
  coordies: [],
  pins: [],
  recents: []
};

export default function engagement(state = initialState, { type, payload }) {
  return produce(state, draft => {
    switch (type) {
      case INITIALIZE:
        Object.keys(initialState).forEach(key => {
          if (payload[key]) {
            draft[key] = engagementViewModelCreator[key](payload[key]);
          }
        });
    }
  });
}
```



즉 내가 초기화하고자 하는 데이터들을 리듀서의 키에 매핑해서 전달하는 형식이다

그렇다면 위 코드에서 `engagementViewModelCreator`는 무엇인가?



```typescript
export const cartsViewModelCreator = (options: IOption[]): ICartViewModel[] => options.map(createCart);

export const engagementViewModelCreator = {
  carts: cartsViewModelCreator,
  recents: recentsViewModelCreator,
  pins: pinsViewModelCreator,
  bookmarks: bookmarksViewModelCreator,
  coordies: coordiesViewModelCreator
};
```



위와 같은 방식으로 서버로부터 받아온 데이터를 받아 initialize 액션을 통해 전달하면 해당 리듀서에서 key를 기준으로 인식하여 약속된 ViewModelCreator로 전달하여 원하는 형태의 데이터로 가공하여 스토어에 데이터를 담아서 UI로 전달하는 형태이다



ViewModelCreator는 아직 그 이름에 비해서 역할이 대단하진 못하다

프로젝트 상황상 axios interceptor에서 snake -> camel을 처리할 수 없어서 새롭게 만들어가는 부분에만 우선 적용하고 있기에 snake -> camel은 ViewModelCreator에서 담당하고 있다

즉 아직은 PreProcessor 단계에 머물러있는 상태이다



또한 개인적인 생각으로 프론트엔드에서 섬세하게 신경써서 코딩하지 않으면 모든 처리가 컴포넌트에 집중되고 그것이 좋지 않다고 생각해서, 최대한 UI 컴포넌트 이전 레이어들을 쌓아 UI 컴포넌트에는 최대한 UI와 관련된 사항들이나 해당 컴포넌트에서만 쓰일 로직들을 보관하는 방향으로 진행하고 있다



그런 측면에서 사소하게는 toLocaleString 부터 computed한 계산값들은 이미 ViewModelCreator 단계에서 최대한 처리해주고자 한다, 또한 서버에서 받아온 데이터라고 해서 우리가 모두 사용하는 것이 아니기에 그런 프로퍼티들은 제외하고 실제로 사용하기 좋도록 정제해주고 있다 (특히나 앱과 API를 공유하고 있어서 서버도 나름대로의 히스토리와 레거시를 유지해야만 하는 부분들이 있어서 그런 간극도 조금씩 메워주고 있다)



다른 사람이 위와 같은 처리를 하는 것을 보고 만든 것은 아니어서 스스로는 항상 불안하게 생각하는 구조지만, 지금 현재는 나름대로 잘 쓰고 있어서 주변 지인들로부터 피드백을 받아 좀 더 나은 방향으로 발전시켜나가고 싶다