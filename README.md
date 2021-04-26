<div>
  <h1 align="center"><a href="https://epicreact.dev/patterns">🤯 Advanced React Patterns 🚀 EpicReact.Dev</a></h1>
  <strong>
    Learn how to build simple and flexible React Components and Hooks using
    modern patterns
  </strong>
  <p>
    Not only learn great patterns you can use but also the strengths and
    weaknesses of each, so you know which to reach for to provide your custom
    hooks and components the flexibility and power you need.
  </p>

  <a href="https://epicreact.dev">
    <img
      alt="Learn React from Start to Finish"
      src="https://kentcdodds.com/images/epicreact-promo/er-1.gif"
    />
  </a>
</div>

<hr />

<!-- prettier-ignore-start -->
[![Build Status][build-badge]][build]
[![All Contributors][all-contributors-badge]](#contributors)
[![GPL 3.0 License][license-badge]][license]
[![Code of Conduct][coc-badge]][coc]
<!-- prettier-ignore-end -->

## Prerequisites

- Read my blog post
  [Inversion of Control](https://kentcdodds.com/blog/inversion-of-control). Or
  watch
  [Implement Inversion of Control](https://egghead.io/lessons/egghead-implement-inversion-of-control?pl=kent-s-blog-posts-as-screencasts-eefa540c&af=5236ad)
- The more experience you have with building React abstractions, the more
  helpful this workshop will be for you.

## System Requirements

- [git][git] v2.13 or greater
- [NodeJS][node] `12 || 14 || 15`
- [npm][npm] v6 or greater

All of these must be available in your `PATH`. To verify things are set up
properly, you can run this:

```shell
git --version
node --version
npm --version
```

If you have trouble with any of these, learn more about the PATH environment
variable and how to fix it here for [windows][win-path] or
[mac/linux][mac-path].

## Setup

> If you want to commit and push your work as you go, you'll want to
> [fork](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/fork-a-repo)
> first and then clone your fork rather than this repo directly.

After you've made sure to have the correct things (and versions) installed, you
should be able to just run a few commands to get set up:

```
git clone https://github.com/kentcdodds/advanced-react-patterns.git
cd advanced-react-patterns
node setup
```

This may take a few minutes. **It will ask you for your email.** This is
optional and just automatically adds your email to the links in the project to
make filling out some forms easier.

If you get any errors, please read through them and see if you can find out what
the problem is. If you can't work it out on your own then please [file an
issue][issue] and provide _all_ the output from the commands you ran (even if
it's a lot).

If you can't get the setup script to work, then just make sure you have the
right versions of the requirements listed above, and run the following commands:

```
npm install
npm run validate
```

If you are still unable to fix issues and you know how to use Docker 🐳 you can
setup the project with the following command:

```
docker-compose up
```

It's recommended you run everything locally in the same environment you work in
every day, but if you're having issues getting things set up, you can also set
this up using [GitHub Codespaces](https://github.com/features/codespaces)
([video demo](https://www.youtube.com/watch?v=gCoVJm3hGk4)) or
[Codesandbox](https://codesandbox.io/s/github/kentcdodds/advanced-react-patterns).

## Running the app

To get the app up and running (and really see if it worked), run:

```shell
npm start
```

This should start up your browser. If you're familiar, this is a standard
[react-scripts](https://create-react-app.dev/) application.

You can also open
[the deployment of the app on Netlify](https://advanced-react-patterns.netlify.app/).

## Running the tests

```shell
npm test
```

This will start [Jest](https://jestjs.io/) in watch mode. Read the output and
play around with it. The tests are there to help you reach the final version,
however _sometimes_ you can accomplish the task and the tests still fail if you
implement things differently than I do in my solution, so don't look to them as
a complete authority.

### Exercises

- `src/exercise/00.md`: Background, Exercise Instructions, Extra Credit
- `src/exercise/00.js`: Exercise with Emoji helpers
- `src/__tests__/00.js`: Tests
- `src/final/00.js`: Final version
- `src/final/00.extra-0.js`: Final version of extra credit

The purpose of the exercise is **not** for you to work through all the material.
It's intended to get your brain thinking about the right questions to ask me as
_I_ walk through the material.

### Helpful Emoji 🐨 💰 💯 📝 🦉 📜 💣 💪 🏁 👨‍💼 🚨

Each exercise has comments in it to help you get through the exercise. These fun
emoji characters are here to help you.

- **Kody the Koala** 🐨 will tell you when there's something specific you should
  do version
- **Marty the Money Bag** 💰 will give you specific tips (and sometimes code)
  along the way
- **Hannah the Hundred** 💯 will give you extra challenges you can do if you
  finish the exercises early.
- **Nancy the Notepad** 📝 will encourage you to take notes on what you're
  learning
- **Olivia the Owl** 🦉 will give you useful tidbits/best practice notes and a
  link for elaboration and feedback.
- **Dominic the Document** 📜 will give you links to useful documentation
- **Berry the Bomb** 💣 will be hanging around anywhere you need to blow stuff
  up (delete code)
- **Matthew the Muscle** 💪 will indicate that you're working with an exercise
- **Chuck the Checkered Flag** 🏁 will indicate that you're working with a final
- **Peter the Product Manager** 👨‍💼 helps us know what our users want
- **Alfred the Alert** 🚨 will occasionally show up in the test failures with
  potential explanations for why the tests are failing.

## Notes

- Context provider/consumers in React DevTools just display as Context.Provider
  and Context.Consumer. That doesn’t do a good job differentiating itself from
  other contexts that may be in your app. Luckily, you can set the context
  displayName and it’ll display that name for the Provider and Consumer:

```js
const MyContext = React.createContext()
MyContext.displayName = 'MyContext'
```

- **Context Module Functions** - This pattern in some situations this pattern
  can not only help you reduce duplication, but it also helps improve
  performance and helps you avoid mistakes in dependency lists

```js
// src/context/counter.js
const CounterContext = React.createContext()

function CounterProvider({step = 1, initialCount = 0, ...props}) {
  const [state, dispatch] = React.useReducer(
    (state, action) => {
      const change = action.step ?? step
      switch (action.type) {
        case 'increment': {
          return {...state, count: state.count + change}
        }
        case 'decrement': {
          return {...state, count: state.count - change}
        }
        default: {
          throw new Error(`Unhandled action type: ${action.type}`)
        }
      }
    },
    {count: initialCount},
  )

  const value = [state, dispatch]

  return <CounterContext.Provider value={value} {...props} />
}

function useCounter() {
  const context = React.useContext(CounterContext)
  if (context === undefined) {
    throw new Error(`useCounter must be used within a CounterProvider`)
  }
  return context
}

const increment = dispatch => dispatch({type: 'increment'})
const decrement = dispatch => dispatch({type: 'decrement'})

export {CounterProvider, useCounter, increment, decrement}

// src/screens/counter.js
import {useCounter, increment, decrement} from 'context/counter'

function Counter() {
  const [state, dispatch] = useCounter()
  return (
    <div>
      <div>Current Count: {state.count}</div>
      <button onClick={() => decrement(dispatch)}>-</button>
      <button onClick={() => increment(dispatch)}>+</button>
    </div>
  )
}
```

_Passing raw dispatch down also makes code splitting such "async methods"
easier. You import them from the leaf component that uses them instead of the
root component. So you only pay for what you use where you use it - Dan Abramov_

A benefit of doing things this way is that, when we have multiple dispatch that
we need to be calling, is very easy for the user to miss the order os miss some
of these calls, so the best is to group into a helper function. Common thing
people do instead is put the helpers inside the consumer hook of the value, but
then you have to memoize and take care about performance and useCallback
dependency list, and in addition this means you can't code splitting or tree
shake this codes into modules functions, it also hurts the ability of lazyling
load this helper functions as well. This is currently a typical patern at
Facebook and many other larger codebases

- **Compound Components** - These are components that works together to form a
  complete UI, an example of that is `<select>` and `<option>` in HTML:

```html
<select>
  <option value="1">Option 1</option>
  <option value="2">Option 2</option>
</select>
```

[https://reach.tech/](Reach UI) components implements this pattern a lot:
[https://reach.tech/tabs/](https://reach.tech/tabs/)

- This pattern is useful because if gives more flexibility to the consumer of
  the API, because who are consuming can define what props or components it will
  have inside the wrapper component and the order, and still have access to the
  state of the component. Also is good for maintaining side as it is less code
  to generate and less use cases to predict. At least, it's implicit from the
  perspective of the users, and it's explicit from the perspective of this
  toggle component.That's how you share implicit state for compound components.

Example:

```js
function Toggle({children}) {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  // instead of returning <Switch on={on} onClick={toggle} />, as it is on most of API,
  // we are iterating over React.Children and returning a cloned version of the chidren overriding
  // the props we want
  return React.Children.map(children, (child, index) => {
    return React.cloneElement(child, {
      toggle,
      on,
    })
  })
}
```

- **React.Children** - get all children in the component
- **React.cloneElement** - cloned version of the component, can override props
  on the second argument

- [**dequal**](https://github.com/lukeed/dequal#readme) is a "deep equal" js
  utility

- A DOM component is a built-in component like `<div />`, `<span />`, or
  `<blink />`. A composite component is a custom component like `<Toggle />` or
  `<App />`.
- A easy way to differentiate between composite component vs DOM component is
  check the type, if `typeof type === 'function'` is true then is composite,
  otherwise it's a DOM component. Or if you preffer the opposite: if
  `typeof type === 'string'` is true then is DOM.
- If you want to prevent adding implicity props for every component, you can
  create a rule like:

```js
const allowedTypes = [ToggleOn, ToggleOff, ToggleButton]

//...
return React.children.map(children, child => {
  if (allowedTypes.includes(child.type)) {
    return React.cloneElement(child, {on, toggle})
  }
  return child
})
//...
```

**Flexible Compound Components** - This pattern is more advanced that his
previous version _Compound Components_ because it allow to pass state to
multiple nested children instead of just the immediate children.

[@reach/accordion](https://reach.tech/accordion/) is an example of Real World
project that uses this pattern

Ex:

```js
//...
const ToggleContext = React.createContext()
//...
function ToggleOn({children}) {
  const {on} = React.useContext(ToggleContext)
  return on ? children : null
}
function ToggleOff({children}) {
  const {on} = React.useContext(ToggleContext)
  return on ? null : children
}
function ToggleButton({...props}) {
  const {on, toggle} = React.useContext(ToggleContext)
  return <Switch on={on} onClick={toggle} {...props} />
}
function App() {
  return (
    <div>
      <Toggle>
        <ToggleOn>The button is on</ToggleOn>
        <ToggleOff>The button is off</ToggleOff>
        <div>
          <ToggleButton />
        </div>
      </Toggle>
    </div>
  )
}
//...
```

Remember to add a helper function, so your context won't be exposed:

```js
const ToggleContext = React.createContext()

const useToggle = () => React.useContext(ToggleContext)

//...
function ToggleOn({children}) {
  const {on} = useToggle()
  return on ? children : null
}
//...
```

- Custom hooks validation: We can always help the user of our custom hook to
  have a better understandingt about our API by providing custom validations:

```js
const ToggleContext = React.createContext()

const useToggle = () => {
  const context = React.useContext(ToggleContext)
  // if context is undefined it means useToggle was called without being wrapped by a ToggleContext.Provider
  if (!context) {
    throw new Error(`useToggle must be used within a Toggle`)
  }
  return context
}
```

- We can also add a better displayName for our contexts, so instead od display
  `<Context.Provider>` in the developer tools, we can have a better name:

```js
//...
const ToggleContext = React.createContext()
ToggleContext.displayName = 'ToggleContext'
//...
```

![image](https://user-images.githubusercontent.com/19270322/113576705-70174a00-95f6-11eb-981f-c63fa4d53b1a.png)

**Prop Collections and Getters** - Techinique used to pass attributes or common
props down to reusable components, usually used with custom hooks. I.e for
accessibility purposes a button functioning as toggle should have the
`aria-pressed` attribute set to `true` or `false` when toggled, also the onClick
handler, so instead of having to remember to pass down that props for every
component we use prop getters.

Real World Projects that use this pattern:

[downshift](https://github.com/downshift-js/downshift) (uses prop getters)
[react-table](https://github.com/tannerlinsley/react-table) (uses prop getters)
[@reach/tooltip](https://reach.tech/tooltip/) (uses prop collections)

_Prop Collection_ Ex:

```js
//...
function useToggle() {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  return {on, toggle, togglerProps: {'aria-pressed': on, onClick: toggle}}
}
//...
function App() {
  const {on, togglerProps} = useToggle()
  return (
    <div>
      {/* <span class="toggle-btn toggle-btn-off" aria-pressed="false"></span> */}
      <Switch on={on} {...togglerProps} />
      <hr />
      {/* <button aria-label="custom-button" aria-pressed="false">off</button> */}
      <button aria-label="custom-button" {...togglerProps}>
        {on ? 'on' : 'off'}
      </button>
    </div>
  )
}
```

_Prop Getter_ Ex:

```js
//...
function useToggle() {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

    const getTogglerProps = ({onClick, on, ...props}) => ({
    onClick: () => {
      toggle()
      // onClick?.() // or
      // onClick: callAll(onClick, toggle), // or²
      typeof onClick === 'function' && onClick()
    },
    on,
    ...props,
  })

  return {
    on,
    toggle,
    getTogglerProps,
  }
}
//...
function App() {
  return (
    <div>
      <Switch {...getTogglerProps({on})} />
      <hr />
      <button
        {...getTogglerProps({
          'aria-label': 'custom-button',
          onClick: () => console.info('onButtonClick'),
          id: 'custom-button-id',
        })}
      >
    </div>
  )
}
```

**getAll function** is a techinique used to compose multiple functions calls,
where we can have any number of functions as arguments, and check if they exists
and call only if exists

I.e:

```js
function callAll(...fns) {
  return (...args) => {
    fns.forEach(fn => {
      fn && fn(...args)
    })
  }
}
```

**State Reducer** - Is a Pattern that provides inversion of control by using
react reducer. Main benefit is avoid the never ending list of logical
customizations that people could want out of our custom hook. So, basically, we
create a default reducer for our hook, but we also allow the user of the hook to
provide his very own custom reducer, so he can extend and add any new
functionality he desires

Real World Projects that use this pattern:

[downshift](https://github.com/downshift-js/downshift)

Ex:

```js
function toggleReducer(state, {type, initialState}) {
  switch (type) {
    case 'toggle': {
      return {on: !state.on}
    }
    case 'reset': {
      return initialState
    }
    default: {
      throw new Error(`Unsupported type: ${type}`)
    }
  }
}

function useToggle({initialOn = false, reducer = toggleReducer} = {}) {
  const {current: initialState} = React.useRef({on: initialOn})

  //....
}

function App() {
  const [timesClicked, setTimesClicked] = React.useState(0)
  const clickedTooMuch = timesClicked >= 4

  function toggleStateReducer(state, action) {
    switch (action.type) {
      case 'toggle': {
        if (clickedTooMuch) {
          return {on: state.on}
        }
        return {on: !state.on}
      }
      case 'reset': {
        return {on: false}
      }
      default: {
        throw new Error(`Unsupported type: ${action.type}`)
      }
    }
  }

  const {on, getTogglerProps, getResetterProps} = useToggle({
    reducer: toggleStateReducer,
  })
  //...
}
```

We can also export our reducer, so the user can extend, without having to
reimplement everything:

```js
const toggleReducerActions = {
  toggle: 'toggle',
  reset: 'reset',
}

function toggleReducer(state, {type, initialState}) {
  switch (type) {
    case toggleReducerActions.toggle: {
      return {on: !state.on}
    }
    case toggleReducerActions.reset: {
      return initialState
    }
    default: {
      throw new Error(`Unsupported type: ${type}`)
    }
  }
}

function toggleStateReducer(state, action) {
  if (action.type === toggleReducerActions.toggle && clickedTooMuch) {
    return {on: state.on}
  }
  return toggleReducer(state, action)
}
```

When using State Reducer, consider creating a Action Types, so we can avoid
typos (or simply use TS :p)

```js
const actionTypes = {
  toggle: 'toggle',
  reset: 'reset',
}
```

**Control Props** - In this pattern we want to let the user decide wheter he
wants to control the value by itself while also supporting the default, so user
can pass his own state and a onChange function, or just use as it is. This
concept is basically the same as controlled form elements in React:

```js
function MyCapitalizedInput() {
  const [capitalizedValue, setCapitalizedValue] = React.useState('')

  return (
    <input
      value={capitalizedValue}
      onChange={e => setCapitalizedValue(e.target.value.toUpperCase())}
    />
  )
}
```

Real World Projects that use this pattern:

[downshift](https://github.com/downshift-js/downshift)
[@reach/listbox](https://reach.tech/listbox/)

```js
function useToggle({
  initialOn = false,
  reducer = toggleReducer,
  onChange,
  on: controlledOn,
} = {}) {
  const {current: initialState} = React.useRef({on: initialOn})
  const [state, dispatch] = React.useReducer(reducer, initialState)
  const onIsControlled = controlledOn != null
  const on = onIsControlled ? controlledOn : state.on
  const dispatchWithOnChange = action => {
    if (!onIsControlled) {
      dispatch(action)
    }
    onChange?.(reducer({...state, on}, action), action)
  }
  const toggle = () => dispatchWithOnChange({type: actionTypes.toggle})
  const reset = () =>
    dispatchWithOnChange({type: actionTypes.reset, initialState})

  function getTogglerProps({onClick, ...props} = {}) {
    return {
      'aria-pressed': on,
      onClick: callAll(onClick, toggle),
      ...props,
    }
  }

  function getResetterProps({onClick, ...props} = {}) {
    return {
      onClick: callAll(onClick, reset),
      ...props,
    }
  }

  return {
    on,
    reset,
    toggle,
    getTogglerProps,
    getResetterProps,
  }
}

function Toggle({on: controlledOn, onChange}) {
  const {on, getTogglerProps} = useToggle({on: controlledOn, onChange})
  const props = getTogglerProps({on})
  return <Switch {...props} />
}
function App() {
  const [bothOn, setBothOn] = React.useState(false)

  const handleToggleChange = (state, action) => setBothOn(state.on)

  return (
//...
  <Toggle on={bothOn} onChange={handleToggleChange} />
  <Toggle
    onChange={(...args) =>
      console.info('Uncontrolled Toggle onChange', ...args)
    }
  />
  //...
  )
}
```

**Variable Shadowing** - In computer programming, variable shadowing occurs when
a variable declared within a certain scope (decision block, method, or inner
class) has the same name as a variable declared in an outer scope. At the level
of identifiers (names, rather than variables), this is known as name masking.
This outer variable is said to be shadowed by the inner variable, while the
inner identifier is said to mask the outer identifier. This can lead to
confusion, as it may be unclear which variable subsequent uses of the shadowed
variable name refer to, which depends on the name resolution rules of the
language.

- use `!= null` to verify if value is not null nor undefined:

```js
const onIsControlled = controlledOn != null
```

**warning** - https://www.npmjs.com/package/warning - this package is a mirror
of Facebook's (FB) warning module used within React's source code, useful when
we want to provide useful console errors but for development only

- check the
  [useControlledSwitchWarning](https://github.com/reach/reach-ui/blob/a376daec462ccb53d33f4471306dff35383a03a5/packages/utils/src/index.tsx#L407-L443)
  for a custom hook for control props warning

## Contributors

Thanks goes to these wonderful people
([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://kentcdodds.com"><img src="https://avatars.githubusercontent.com/u/1500684?v=3?s=100" width="100px;" alt=""/><br /><sub><b>Kent C. Dodds</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=kentcdodds" title="Code">💻</a> <a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=kentcdodds" title="Documentation">📖</a> <a href="#infra-kentcdodds" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a> <a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=kentcdodds" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/FWeinb"><img src="https://avatars0.githubusercontent.com/u/1250430?v=4?s=100" width="100px;" alt=""/><br /><sub><b>FWeinb</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/issues?q=author%3AFWeinb" title="Bug reports">🐛</a> <a href="#ideas-FWeinb" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/dlannoye"><img src="https://avatars2.githubusercontent.com/u/1383720?v=4?s=100" width="100px;" alt=""/><br /><sub><b>David Lannoye</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/issues?q=author%3Adlannoye" title="Bug reports">🐛</a> <a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=dlannoye" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/colinrcummings"><img src="https://avatars2.githubusercontent.com/u/9815009?s=460&v=4?s=100" width="100px;" alt=""/><br /><sub><b>Colin Cummings</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=colinrcummings" title="Code">💻</a> <a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=colinrcummings" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/bkoltai"><img src="https://avatars2.githubusercontent.com/u/464764?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Benji Koltai</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=bkoltai" title="Documentation">📖</a></td>
    <td align="center"><a href="http://baggasumit.github.io"><img src="https://avatars1.githubusercontent.com/u/1779959?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sumit Bagga</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=baggasumit" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/Tarabyte"><img src="https://avatars0.githubusercontent.com/u/2027010?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Yury Tarabanko</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=Tarabyte" title="Code">💻</a></td>
  </tr>
  <tr>
    <td align="center"><a href="http://www.wendtedesigns.com/"><img src="https://avatars2.githubusercontent.com/u/5779538?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alex Wendte</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=themostcolm" title="Code">💻</a></td>
    <td align="center"><a href="https://twitter.com/CompuIves"><img src="https://avatars3.githubusercontent.com/u/587016?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ives van Hoorne</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=CompuIves" title="Code">💻</a> <a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=CompuIves" title="Tests">⚠️</a></td>
    <td align="center"><a href="http://team.thebrain.pro"><img src="https://avatars1.githubusercontent.com/u/4002543?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Łukasz Gandecki</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=lgandecki" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/deniztetik"><img src="https://avatars0.githubusercontent.com/u/14167019?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Deniz Tetik</b></sub></a><br /><a href="#content-deniztetik" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Ruffeng"><img src="https://avatars1.githubusercontent.com/u/18511772?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ruffeng</b></sub></a><br /><a href="#content-Ruffeng" title="Content">🖋</a> <a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=Ruffeng" title="Code">💻</a></td>
    <td align="center"><a href="https://stackshare.io/jdorfman/decisions"><img src="https://avatars1.githubusercontent.com/u/398230?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Justin Dorfman</b></sub></a><br /><a href="#fundingFinding-jdorfman" title="Funding Finding">🔍</a></td>
    <td align="center"><a href="http://alexmunoz.github.io"><img src="https://avatars3.githubusercontent.com/u/3093946?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alex Munoz</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=AlexMunoz" title="Documentation">📖</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/marcosvega91"><img src="https://avatars2.githubusercontent.com/u/5365582?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Marco Moretti</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=marcosvega91" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/emipc"><img src="https://avatars1.githubusercontent.com/u/26004903?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Emili</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=emipc" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/balavishnuvj"><img src="https://avatars3.githubusercontent.com/u/13718688?v=4?s=100" width="100px;" alt=""/><br /><sub><b>balavishnuvj</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=balavishnuvj" title="Code">💻</a></td>
    <td align="center"><a href="https://www.linkedin.com/in/pritamsangani/"><img src="https://avatars3.githubusercontent.com/u/22857896?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Pritam Sangani</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=PritamSangani" title="Code">💻</a></td>
    <td align="center"><a href="http://linkedin.com/in/katarzynakosturek/"><img src="https://avatars3.githubusercontent.com/u/36547835?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kasia Kosturek</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=kocvrek" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/emzoumpo"><img src="https://avatars2.githubusercontent.com/u/2103443?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Emmanouil Zoumpoulakis</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=emzoumpo" title="Documentation">📖</a></td>
    <td align="center"><a href="http://peter.hozak.info/"><img src="https://avatars0.githubusercontent.com/u/1087670?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Peter Hozák</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=Aprillion" title="Code">💻</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/nawok"><img src="https://avatars3.githubusercontent.com/u/159773?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Pavel Fomchenkov</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=nawok" title="Documentation">📖</a></td>
    <td align="center"><a href="http://www.seemaullal.com"><img src="https://avatars0.githubusercontent.com/u/8728285?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Seema Ullal</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=seemaullal" title="Documentation">📖</a></td>
    <td align="center"><a href="https://git.io/JfYj5"><img src="https://avatars0.githubusercontent.com/u/25733135?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Patrick Clery</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=patrickclery" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/degeens"><img src="https://avatars2.githubusercontent.com/u/33414262?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Stijn Geens</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=degeens" title="Documentation">📖</a></td>
    <td align="center"><a href="https://michaeldeboey.be"><img src="https://avatars3.githubusercontent.com/u/6643991?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Michaël De Boey</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=MichaelDeBoey" title="Code">💻</a></td>
    <td align="center"><a href="https://www.daleseo.com"><img src="https://avatars1.githubusercontent.com/u/5466341?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Dale Seo</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=DaleSeo" title="Documentation">📖</a></td>
    <td align="center"><a href="http://bobbywarner.com"><img src="https://avatars0.githubusercontent.com/u/554961?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Bobby Warner</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=bobbywarner" title="Code">💻</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://www.sophiabrandt.com"><img src="https://avatars0.githubusercontent.com/u/16630701?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sophia Brandt</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=sophiabrandt" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/ph08n1x"><img src="https://avatars.githubusercontent.com/u/4249732?v=4?s=100" width="100px;" alt=""/><br /><sub><b>ph08n1x</b></sub></a><br /><a href="https://github.com/kentcdodds/advanced-react-patterns/commits?author=ph08n1x" title="Documentation">📖</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the
[all-contributors](https://github.com/kentcdodds/all-contributors)
specification. Contributions of any kind welcome!

## Workshop Feedback

Each exercise has an Elaboration and Feedback link. Please fill that out after
the exercise and instruction.

At the end of the workshop, please go to this URL to give overall feedback.
Thank you! https://kcd.im/arp-ws-feedback

<!-- prettier-ignore-start -->
[npm]: https://www.npmjs.com/
[node]: https://nodejs.org
[git]: https://git-scm.com/
[build-badge]: https://img.shields.io/github/workflow/status/kentcdodds/advanced-react-patterns/validate/main?logo=github&style=flat-square
[build]: https://github.com/kentcdodds/advanced-react-patterns/actions?query=workflow%3Avalidate
[license-badge]: https://img.shields.io/badge/license-GPL%203.0%20License-blue.svg?style=flat-square
[license]: https://github.com/kentcdodds/advanced-react-patterns/blob/main/LICENSE.md
[coc-badge]: https://img.shields.io/badge/code%20of-conduct-ff69b4.svg?style=flat-square
[coc]: https://github.com/kentcdodds/advanced-react-patterns/blob/main/CODE_OF_CONDUCT.md
[emojis]: https://github.com/kentcdodds/all-contributors#emoji-key
[all-contributors]: https://github.com/kentcdodds/all-contributors
[all-contributors-badge]: https://img.shields.io/github/all-contributors/kentcdodds/advanced-react-patterns?color=orange&style=flat-square
[win-path]: https://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/
[mac-path]: http://stackoverflow.com/a/24322978/971592
[issue]: https://github.com/kentcdodds/advanced-react-patterns/issues/new
<!-- prettier-ignore-end -->
