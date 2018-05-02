---
layout: post
title: "React 입문"
subtitle: "React"
categories: React
tags: React
---

# React 입문

미리 밝히자면 아래 내용은 결과론적일 수 있고, 필자의 개인적인 추측들이 담겨있으며, 마지막으로 필자는 Angular 또는 Vue로 프로젝트를 해본 경험이 전혀 없습니다.





React는 Anti-Angular 목적이 담긴 라이브러리였다고 생각합니다.



예전에는 React에서 내세웠던 3가지 가치가 아래와 같았습니다.



`Just the UI`, `Virtual DOM`, `Data Flow`



첫 번째 내세운 가치인 Just the UI 는, Angular가 풀려는 문제가 매우 크고 그에 따른 해결책도 복잡했기에, React는 매우 단순하며 쉬운 `라이브러리`라는 점을 대단히 강조하고 싶었던 것 같습니다.



두 번째인 Virtual DOM은 Angular가 Digest Loop를 통해 변경사항을 반영했다면, React는 Virtual DOM을 통해서 효과적으로 Diff를 반영한다는 것을 내세우고 싶었던 것 같습니다.



마지막으로 Data Flow 역시 Angular는 Model-View 간의 양방향 바인딩을 지원했던 것에 비해, React는 단방향으로 Data의 변화를 View에 반영시키는 점도 Angular의 복잡성에 대한 차별성으로 나온 것이 아닐까 싶습니다.





최근에 React 공식 사이트가 Gatsby로 개편되면서 내용에도 일부 변화가 있었는데, 내세우는 3가지 가치가 아래와 같이 바뀌었습니다.



`Declarative`, `Component-Based`, `Learn Once, Write Anywhere`



첫 번째에서 말하는, 선언적인 View가 가능하다는 점은 제가 생각하는 React의 가장 큰 장점입니다.

Vanila나 jQuery로 개발했다면, Event Listener를 Delegation해놓고, 해당하는 Event Handler를 절차적으로 작성하여 User Interaction 또는 Data의 변화에 따른 View 수정을 했어야 했을 것입니다.

그에 비해서 React는 Component에 작성해놓은 방향대로 변화(User Interaction, Data Change)가 View에 반영되도록 돕습니다.



두 번째에서 말하는 Component는 굳이 언급이 필요했는지에 대해 잘 모르겠습니다. (React의 고유한 특징이라고 보기는 어려우므로) 오히려 하단에 적혀있는 아래 문구에 좀 더 방점이 찍혀있는 것인가 싶습니다.

> Since component logic is written in JavaScript instead of templates, you can easily pass rich data through your app and keep state out of the DOM.



마지막으로 Learn Once, Write Anywhere 역시 의미를 알기 어려웠는데, React Native 또는 React VR 등을 염두해두고 작성한 것인가 싶습니다.



어떤 라이브러리나 프레임워크를 사용할지 여부를 판단하기 위해 꼭 참고해야 할 Marketing Copy를 기준으로 React를 돌아보았습니다. 제가 알지 못하는 뒷배경들이 많겠지만, 적어도 제게는 예전 문구들이 훨씬 더 매력적이었던 것 같습니다.



실제로 React를 사용하기 위해선 Props, State, setState, Presentation Component, Container Component, Life Cycle, State Management, CRA, Webpack, SSR 등 알아야 할 것들이 많지만 해당 요소들은 다른 글을 통해 알아보기로 하겠습니다.



글에 대한 피드백은 언제든 환영합니다! :D
