# Getting Started With rx-react-store {#getting-started-with-code-redux-form-code-}

The basic implementation of rx-react-store is simple. However, to make the most of it, it's recommended to have basic knowledge on:

* [React](https://facebook.github.io/react/)
* [Higher-Order Components \(HOCs\)](https://facebook.github.io/react/docs/higher-order-components.html)
* Flux
* RxJs

## Installation

```batch
npm i --save @zazapeta/rx-react-store
```

## Data flow

 #### 1. You call `store.dispatch(reducer, ...rest)`
 A `reducer` is a ** pure function **. It only computes the next state. It should be completely predictable: calling it with the same inputs many times should produce the same outputs. It shouldn't perform any side effects like API calls or router transitions. These should happen before an action is dispatched.

 
 You can call store.dispatch(reducer, ...rest) from anywhere in your app, including components and XHR callbacks, or even at scheduled intervals.
 
 #### 2. The store calls the `reducer` function you gave it
 
 The store will pass `n` arguments to the reducer: the first is the current state, rest of arguments are passing with `...rest`. For example : 
 
 ```js
 function setAppCfg(state, title, theme){
  return { ...state, title, theme };
 }
 
 /*
  dipatch : store.dispatch(setTitle, 'My new title', 'dark')
 */
 ```

 #### 3. The store save the new state given by `reducer`
 Every connected registered with `store.connect()(component)` will now be rerender.
 
 >Because `component.setState` is async, the `store.dispatch` is also async by nature.

## Basic starting guide

* Store setup : 
_app.store.js_

```js
import RxStore from '@zazapeta/rx-react-store';

let store = new RxStore();

// setup middlewares
store.AfterGlobalParalel.set('InfoLogger', (state, reducer) =>
  console.info(`[${reducer.name}] STATE:`, state),
);

export default store;
```
* Reducers
_app.reducers.js_
```js
export function setStitle(state, title){
 return {...state, title };
}
```
* Dispatchers
_app.dispatchers.js_
```js
import appStore from './app.store.js';
import * as appReducers from './app.reducers.js';

export default appStore.createDispatchers(appReducers);

```
* Connected Component \(aka Container Component\)
_App.container.jsx_
```js
import appStore from './app.store.js';

function App({title}){
 return (
  <div>
   <h1>{title}</h1>
  </div>
 );
}

export default appStore.connect()(App);
```

_AnOther.container.jsx_
```js
import appDispatchers from './app.dispatchers.js';

export default function InputTitle(){
 return <input type='text' onBlur={(e) => appDispatchers.setTitle(e.target.value)} />
}

```
Each time the input will be blured, App.container.jsx will rerender with the new value of the `title input`. Throttle implemntation is up to you.