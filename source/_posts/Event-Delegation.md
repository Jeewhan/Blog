---
layout: post
title: "이벤트 델리게이션"
subtitle: "자바스크립트"
categories: JS
tags: JS
date: 2017-10-08
---

### Concept
  - Javascript Event
      - addEventListener
    - Event Delegation
      - Event Bubbling, Event Capturing
    - Event Object
      - Event Target


---

### Content

SPA 이전 웹사이트들에서 자바스크립트의 중요한 역할은 웹사이트의 동작을 구현하는 것입니다. 여기서 말하는 동작은 애니메이션이 아닌, 동적인 상호작용을 뜻합니다. (애니메이션은 CSS3의 역할이라고 생각합니다.)



위와 같은 동적인 상호작용을 `Event`라고 지칭합니다. 사용자의 이벤트 요청을 받아들이기 위해서는 사용자의 이벤트 요청을 받아들이기 위한 장치를 마련해두어야 합니다.



이를 위한 가장 일반적인 방법이 `addEventListener()`입니다. `addEventListener()`의 대상이 될 수 있는 `EventTarget`은 `Element`, `Document`, `Window`와 같은 이벤트를 지원하는 객체입니다. 일반적으로는 `document.querySelector()`를 통해 특정 `Element`를 지칭하여 `addEventListener()`를 호출합니다.





IE9부터 사용 가능한 `addEventListener()`는 다음과 같은 인자를 갖고 있습니다. [addEventListener MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

`type`, `listener`, `options || useCapture`, `wantsUntrusted`



- `type`

`EventTarget`에 어떤 `Event`가 일어났을 때, `listener parameter`를 호출할지 결정하는 문자열입니다. 어떤 종류가 있는지는 다음 페이지에서 확인할 수 있습니다. [Event reference MDN](https://developer.mozilla.org/en-US/docs/Web/Events) 일반적으로 많이 사용하는 것은 `click`, `load` 등입니다.



- `listener()`

`type`에서 정의한 `event`가 발생했을 때 호출될 객체입니다. 일반적으로 자바스크립트 함수로 합니다. 이 함수를 웹 브라우저가 호출하면서 인수로 `Event` 객체를 넘겨줍니다. 이 인자를 일반적으로 `e` 또는 `event`로 정의합니다. 가장 많이 사용하는 것은 `Event` 의 target property입니다.



`listener`가 `return false`를 반환할 경우, `preventDefault()`를 한 것과 동일한 효과를 보입니다. `preventDefault()`는 해당 `Node`의 기본 동작을 막는 함수입니다. 예를 들어 `<a>`에서 `EventListener`가 호출되었을 때는 `listener`와 더불어 `<a>`의 본래 역할인 URL 연결도 수행하는데, 이를 막을 수 있습니다.



- `options` `||`  `useCapture`

인수가 `object`이면 options를, `boolean`을 넘겨주면 useCapture로 인식합니다. [WICG Link](https://github.com/WICG/EventListenerOptions/blob/gh-pages/explainer.md#eventlisteneroptions)



- `useCaputre`일 때 (default `false`)


이 인자는 `Capture Phase`일 때의 `Event` 수신 여부를 결정합니다.

false일 경우 이벤트가 발생한 `node`로부터 `window`까지 `Event`가 전파되는 시점에 `Event`를 받게 됩니다. `Event Bubbling`이라고 합니다.

true일 경우 `Window`부터 이벤트가 발생한 `node`까지 `Event`가 전파되는 시점에 `Event`를 받게 됩니다. 이것을 `Event Capturing`이라고 합니다.



- `options`일 때

`capture`: `Capturing` 유무

`once`: `true`를 넘겨주면, `EventListener`를 일회성으로 만들 수 있습니다.

`passive`: `listener`에서의 `preventDefault` 호출 허용 여부를 결정합니다.

`mozSystemGroup`



![img](https://www.w3.org/TR/DOM-Level-3-Events/images/eventflow.svg)



[Codepen 예제](https://codepen.io/jeewhan/pen/XeaXGJ)

---


그렇다면 왜 이벤트 위임을 해야 하는가?



하나의 부모 `Node` 밑에 있는 자식 `Node`들이 있다고 가정해보겠습니다. 해당 자식 `Node`들은 모두 동일한 `Event`를 발생시키길 기대합니다.



이벤트 위임을 하지 않을 경우, 자식 `Node`마다 `Event Listener`를 추가해주어야 하는 것은 물론, 이후에 자식 `Node`가 늘어날 때마다 `Event Listener`도 추가해주어야 합니다. 이렇게 하게 되면 성능도 안 좋겠지만, 프로그래머 입장에서도 매우 번거롭습니다.



그래서 자식 `Node`마다 `Event Listener`를 부여하지 않고, 그들의 부모 `Node`에게만 `Event Listener`를 부여하고, 자식에서 발생하는 `Event`를 받아 처리하도록 하는 것을  이벤트 위임이라고 합니다. 그러면 자식 `Node`가 늘어날 때마다 `Event Listener`를 늘릴 필요없이 부모의 `Event Listener`의 `listener`에 분기 처리만 해주면 됩니다.





이벤트 위임에는 당연히 이벤트 전파가 전제되어 있습니다. 따라서 프로그래머가 의도한 방향대로 동작하게 하려면 이벤트 전파를 제어할 수 있어야 합니다. 그러기 위해서 `stopPropagation()` `stopImmediatePropagation()` 를 사용할 수 있습니다.



- `stopPropagation()`
  `Event`가 `Parent Node`로 전파되지 않도록 합니다. 이 함수의 적용 시점은 `Bubbling Phase`이기에, `Capture`와는 무관합니다.




- `stopImmediatePropagation()`
  `stopPropagation()`와 더불어 `Sibling Node`에게도 `Event`가 전달되지 않도록 합니다.


---


### Reference

- [Javascript에서 이벤트 전파를 중단하는 네 가지 방법](http://programmingsummaries.tistory.com/313)
- [왜 이벤트 위임(delegation)을 해야 하는가?](https://github.com/nhnent/fe.javascript/wiki/August-22-August-26,-2016)
- [PoiemaWeb Javascript Event](http://poiemaweb.com/js-event)
