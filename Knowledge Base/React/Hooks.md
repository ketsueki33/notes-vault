Hooks are functions that let you “hook into” React state and lifecycle features.

In simple words, a **Hook** is a function that **allows us to tap into built in features in React** and gives you access to **React's internal Memory**.

All hooks names start with `use`.
### Rule of Hooks
We must remember these guidelines when using hooks.

##### 1. Only call Hooks at the top level
**Don’t call Hooks inside loops, conditions, nested functions, or `try`/`catch`/`finally` blocks.** Instead, always use Hooks at the top level of your React function, before any early returns. You can only call Hooks while React is rendering a function component:

- ✅ Call them at the top level in the body of a function component
- ✅ Call them at the top level in the body of a [[#Custom Hooks|custom hook]].

The reason for this rule is that **React relies on the order in which hooks are called** to correctly preserve the state and behavior of your components between renders. If you call hooks inside loops or conditions, the order in which they are called could change between renders, leading to bugs.

##### 2. Only call Hooks from React functions
Don’t call Hooks from regular JavaScript functions. Instead, you can:

- ✅ Call Hooks from React function components.
- ✅ Call Hooks from [[#Custom Hooks|custom hook]].

By following this rule, you ensure that all stateful logic in a component is clearly visible from its source code.

### In-Built Hooks
React has many in-built hooks. To use them, simply import them from `react`.
```tsx
import { useState } from "react";
```
#### useState
`useState` is a React Hook that lets you add a state variable to your component.

> [!Info] What is state?
> Components often need to change what’s on the screen as a result of an interaction.
> 
> Typing into the form should update the input field, clicking “next” on an image carousel should change which image is displayed, clicking “buy” should put a product in the shopping cart. Components need to “remember” things: the current input value, the current image, the shopping cart.
> 
> In React, this kind of component-specific memory is called _state_.

To initialize a state variable: 
```tsx
const [state, setState] = useState(initialState);
```

The convention is to name state variables like `[something, setSomething]` using array destructuring.

`useState` returns an array with exactly two values:

1. The current state. During the first render, it will match the `initialState` you have passed.
2. The `set` function that lets you update the state to a different value and trigger a re-render.

*A component re-renders every time it's state changes.*

**Example Usage:**
```tsx
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      You pressed me {count} times
    </button>
  );
}
```

###### Updating State based on previous state

> [!warning] Batching of State Updates
> React optimizes performance by batching multiple state updates into a single render cycle. This means when multiple `setState` calls happen in quick succession (in a synchronous event handler, for example), React doesn't immediately re-render after each `setState`. Instead, it waits, batches them together, and performs a single render with the final state.

Suppose the `age` is `42`. This handler calls `setAge(age + 1)` three times:
```tsx
function handleClick() {
  setAge(age + 1); // setAge(42 + 1)
  setAge(age + 1); // setAge(42 + 1)
  setAge(age + 1); // setAge(42 + 1)
}
```
However, after one click, `age` will only be `43` rather than `45`. This is because React batches the state update, the age doesn't change till the next render. So each `setAge(age + 1)` call becomes `setAge(43)`.

To solve this problem, **you can pass an _updater function_** to `setAge` instead of the next state:
```tsx
function handleClick() {
  setAge(prev => prev + 1); // setAge(42 => 43)
  setAge(prev => prev + 1); // setAge(43 => 44)
  setAge(prev => prev + 1); // setAge(44 => 45)
}
```

Here, `prev => prev + 1` is your updater function. It takes the pending state and calculates the next state from it.

React puts your updater functions in a queue. Then, during the next render, it will call them in the same order:

1. `prev => prev + 1` will receive `42` as the pending state and return `43` as the next state.
2. `prev => prev + 1` will receive `43` as the pending state and return `44` as the next state.
3. `prev => prev + 1` will receive `44` as the pending state and return `45` as the next state.

There are no other queued updates, so React will store `45` as the current state in the end.

###### Initializing a state with a function
It is possible to pass a function that returns the initial value to `useState`. 

```tsx
function TodoList() {
  const [todos, setTodos] = useState(createInitialTodos);
  // ...
```
Notice that you’re passing `createInitialTodos`, which is the _function itself_, and not `createInitialTodos()`, which is the result of calling it. If you pass a function to `useState`, **React will only call it during initialization.**
### Custom Hooks
