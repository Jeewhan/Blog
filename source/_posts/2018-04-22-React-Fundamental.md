---
layout: post
title: "React Fundamental"
subtitle: "React"
categories: React
tags: React
---

# Why React?

Composition Model

- A good function should follow the "Do One Thing" rule
- When you combine these simple functions together to form a more complex function, this is **composition**.
- [Compose me That: Function Composition in JavaScript](https://www.linkedin.com/pulse/compose-me-function-composition-javascript-kevin-greene/)
- [Functional JavaScript: Function lComposition For Every Day Use.](https://hackernoon.com/javascript-functional-composition-for-every-day-use-22421ef65a10)

Declarative Code

- When JavaScript code is written *imperatively*, we tell JavaScript exactly **what** to do and **how** to do it. Think of it as if we're giving JavaScript *commands* on exactly what steps it should take.
- we *declare* what we want done, and JavaScript will take care of doing it.
- *Imperative* code instructs JavaScript on *how* it should perform each step. With *declarative* code, we tell JavaScript *what* we want to be done, and let JavaScript take care of performing the steps.
- [Imperative vs Declarative Programming](https://tylermcginnis.com/imperative-vs-declarative-programming/)
- [Difference between declarative and imperative in React.js?](https://stackoverflow.com/questions/33655534/difference-between-declarative-and-imperative-in-react-js) from StackOverflow

Unidirectional Data Flow

- *Data flows down from parent component to child component. Data updates are sent to the parent component where the parent performs the actual change.* this might seem like extra work, but having the data flow in one direction and having one place where the data is modified makes it much easier to understand how the application works.
- In React, data flows in only one direction, from parent to child. If data is shared between sibling child components, then the data should be stored in the parent component and passed to both of the child components that need it.

Just JavaScript

- Why not look through some of your existing code and try converting your `for` loops to `.map()` calls or see if you can remove any `if` statements by using `.filter()`.



# Rendering UI with React

Creating Elements and JSX

- ```javascript
  React.createElement( /* type */, /* props */, /* content */ );
  ```

- Let's break down what each item can be:

  - `type` – either a string or a React Component

    This can be a string of any existing HTML element (e.g. `'p'`, `'span'`, or `'header'`) or you could pass a React *component* (we'll be creating components with JSX, in just a moment).


  - `props` – either `null` or an object

    This is an object of HTML attributes and custom data about the element.


  - `content` – `null`, a string, a React Element, or a React Component

    Anything that you pass here will be the content of the rendered element. This can include plain text, JavaScript code, other React elements, etc.
