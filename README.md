# react-hanger

[![npm version](https://badge.fury.io/js/react-hanger.svg)](https://badge.fury.io/js/react-hanger)
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-2-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

Set of a helpful hooks, for different specific to some primitives types state changing helpers.

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="http://liveflow.io"><img src="https://avatars.githubusercontent.com/u/3940079?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Andrey Los</b></sub></a><br /><a href="#ideas-RIP21" title="Ideas, Planning, & Feedback">🤔</a> <a href="#infra-RIP21" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a> <a href="https://github.com/kitze/react-hanger/commits?author=RIP21" title="Tests">⚠️</a> <a href="https://github.com/kitze/react-hanger/commits?author=RIP21" title="Code">💻</a></td>
    <td align="center"><a href="https://praneet.dev"><img src="https://avatars.githubusercontent.com/u/23721710?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Praneet Rohida</b></sub></a><br /><a href="#infra-praneetrohida" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a> <a href="https://github.com/kitze/react-hanger/commits?author=praneetrohida" title="Tests">⚠️</a> <a href="https://github.com/kitze/react-hanger/commits?author=praneetrohida" title="Code">💻</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!

### Check out:

- 💻 [Sizzy](https://sizzy.co) - The Browser For Developers
- 🔮 [Fungarzione](https://fungarzione.co) - Keep your users in the loop (Changelogs, Roadmap, Issues)
- 💌 [Twizzle](https://twizzle.app) - A standalone app for Twitter DM

---


Has two APIs:

- [First](#Example) and original from v1 is based on object destructuring e.g. `const { value, toggle } = useBoolean(false)` (Docs below)
- [Second API](./README-ARRAY.md) (recommended [why?](./README-ARRAY.md#migration-from-object-to-array-based)) is based on more idiomatic to React hooks API, e.g. like `useState` with array destructuring
  `const [value, actions] = useBoolean(false)` [(Docs)](./README-ARRAY.md)

## Install

```bash
yarn add react-hanger
```

## Usage

```jsx
import React, { Component } from 'react';

import { useInput, useBoolean, useNumber, useArray, useOnMount, useOnUnmount } from 'react-hanger';

const App = () => {
  const newTodo = useInput('');
  const showCounter = useBoolean(true);
  const limitedNumber = useNumber(3, { lowerLimit: 0, upperLimit: 5 });
  const counter = useNumber(0);
  const todos = useArray(['hi there', 'sup', 'world']);

  const rotatingNumber = useNumber(0, {
    lowerLimit: 0,
    upperLimit: 4,
    loop: true,
  });

  return (
    <div>
      <button onClick={showCounter.toggle}> toggle counter </button>
      <button onClick={() => counter.increase()}> increase </button>
      {showCounter.value && <span> {counter.value} </span>}
      <button onClick={() => counter.decrease()}> decrease </button>
      <button onClick={todos.clear}> clear todos </button>
      <input type="text" value={newTodo.value} onChange={newTodo.onChange} />
    </div>
  );
};
```

### Example

[![Edit react-hanger example](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/react-hanger-example-ize89?fontsize=14&hidenavigation=1&theme=dark)

## API reference (object destructuring)

### How to import?

```
import { useBoolean } from 'react-hanger' // will import all of functions
import useBoolean from 'react-hanger/useBoolean' // will import only this function
```

### useStateful

Just an alternative syntax to `useState`, because it doesn't need array destructuring.
It returns an object with `value` and a `setValue` method.

```jsx
const username = useStateful('test');

username.setValue('tom');
console.log(username.value);
```

### useBoolean

```jsx
const showCounter = useBoolean(true);
```

Methods:

- `toggle`
- `setTrue`
- `setFalse`

### useNumber

```jsx
const counter = useNumber(0);
const limitedNumber = useNumber(3, { upperLimit: 5, lowerLimit: 3 });
const rotatingNumber = useNumber(0, {
  upperLimit: 5,
  lowerLimit: 0,
  loop: true,
});
```

Methods:

Both `increase` and `decrease` take an optional `amount` argument which is 1 by default, and will override the `step` property if it's used in the options.

- `increase(amount = 1)`
- `decrease(amount = 1 )`

Options:

- `lowerLimit`
- `upperLimit`
- `loop`
- `step` - sets the increase/decrease amount of the number upfront, but it can still be overriden by `number.increase(3)` or `number.decrease(5)`

### useInput

```jsx
const newTodo = useInput('');
```

```jsx
<input value={newTodo.value} onChange={newTodo.onChange} />
```

```jsx
<input {...newTodo.eventBind} />
<Slider {...newTodo.valueBind} />
```

Methods:

- `clear`
- `onChange`
- `eventBind` - binds the `value` and `onChange` props to an input that has `e.target.value`
- `valueBind` - binds the `value` and `onChange` props to an input that's using only `value` in `onChange` (like most external components)

Properties:

- `hasValue`

### useArray

```jsx
const todos = useArray([]);
```

Methods:

- `push` - similar to native doesn't return length tho
- `unshift` - similar to native doesn't return length tho
- `pop` - similar to native doesn't return element tho
- `shift` - similar to native doesn't return element tho
- `clear`
- `removeIndex`
- `removeById` - if array consists of objects with some specific `id` that you pass
  all of them will be removed
- `modifyById` - if array consists of objects with some specific `id` that you pass
  these elements will be modified.
- `move` - moves item from position to position shifting other elements.

```
    So if input is [1, 2, 3, 4, 5]

    from  | to    | expected
    3     | 0     | [4, 1, 2, 3, 5]
    -1    | 0     | [5, 1, 2, 3, 4]
    1     | -2    | [1, 3, 4, 2, 5]
    -3    | -4    | [1, 3, 2, 4, 5]
```

### useMap

```jsx
const { value, set } = useMap([['key', 'value']]);
const { value: anotherValue, remove } = useMap(new Map([['key', 'value']]));
```

Actions:

- `set`
- `delete`
- `clear`
- `initialize` - applies tuples or map instances
- `setValue`

### useSet

```jsx
const set = useSet(new Set<number>([1, 2]));
```

### useSetState

```jsx
const { state, setState, resetState } = useSetState({ loading: false });
setState({ loading: true, data: [1, 2, 3] });
resetState();
```

Methods:

- `setState(value)` - will merge the `value` with the current `state` (like this.setState works in React)
- `resetState()` - will reset the current `state` to the `value` which you pass to the `useSetState` hook

Properties:

- `state` - the current state

### usePrevious

Use it to get the previous value of a prop or a state value.
It's from the official [React Docs](https://reactjs.org/docs/hooks-faq.html#how-to-get-the-previous-props-or-state).
It might come out of the box in the future.

```jsx
const Counter = () => {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);
  return (
    <h1>
      Now: {count}, before: {prevCount}
    </h1>
  );
};
```

### usePageLoad

```jsx
const isPageLoaded = usePageLoad();
```

### useScript

```jsx
const { ready, error } = useScript({
  src: 'https://example.com/script.js',
  startLoading: true,
  delay: 100,
  onReady: () => {
    console.log('Ready');
  },
  onError: (error) => {
    console.log('Error loading script ', error);
  },
});
```

### useDocumentReady

```jsx
const isDocumentReady = useDocumentReady();
```

### useGoogleAnalytics

```jsx
useGoogleAnalytics({
  id: googleAnalyticsId,
  startLoading: true,
  delay: 500,
});
```

### useWindowSize

```jsx
const { width, height } = useWindowSize();
```

### useDelay

```jsx
const done = useDelay(1000);
```

### usePersist

```jsx
const tokenValue = usePersist('auth-token', 'value');
```

### useToggleBodyClass

```jsx
useToggleBodyClass(true, 'dark-mode');
```

### useOnClick

```jsx
useOnClick((event) => {
  console.log('Click event fired: ', event);
});
```

### useOnClickOutside

```jsx
// Pass ref to the element
const containerRef = useOnClickOutside(() => {
  console.log('Clicked outside container');
});
```

### useFocus

```jsx
// pass ref to the element
// call focusElement to focus the element
const [elementRef, focusElement] = useFocus();
```

### useImage

```jsx
const { imageVisible, bindToImage } = useImage(src, onLoad, onError);
```

## Migration from v1 to v2

- Migration to array based API is a bit more complex but recommended (especially if you're using ESLint rules for hooks).
  Take a look at [this section](./README-ARRAY.md#migration-from-object-to-array-based) in array API docs.
- All lifecycle helpers are removed. Please replace `useOnMount`, `useOnUnmount` and `useLifecycleHooks` with `useEffect`.
  This:

```javascript
useOnMount(() => console.log("I'm mounted!"));
useOnUnmount(() => console.log("I'm unmounted"));
// OR
useLifecycleHooks({
  onMount: () => console.log("I'm mounted!"),
  onUnmount: () => console.log("I'm unmounted!"),
});
```

to:

```javascript
useEffect(() => {
  console.log("I'm mounted!");
  return () => console.log("I'm unmounted");
}, []);
```

- `bind` and `bindToInput` are got renamed to `valueBind` and `eventBind` respectively on `useInput` hook
