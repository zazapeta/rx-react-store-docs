# Getting Started With rx-react-store {#getting-started-with-code-redux-form-code-}

The basic implementation of rx-react-store is simple. However, to make the most of it, it's recommended to have basic knowledge on:

* [React](https://facebook.github.io/react/)
* [Higher-Order Components \(HOCs\)](https://facebook.github.io/react/docs/higher-order-components.html)
* Flux
* RxJs

## Data flow

 #### 1. You call `store.dispatch(reducer, ...rest)`
 A `reducer` is a ** pure function ** that, given a state, return a new new state.
 
 You can call store.dispatch(reducer, ...rest) from anywhere in your app, including components and XHR callbacks, or even at scheduled intervals.
 
 #### 2. The store calls the `reducer` function you gave it
 
 The store will pass `n` arguments to the reducer: the first is the current state, rest of arguments are passing with `...rest`. For example : 
 
 ```js
 function setTitle(state, title){
  return { ...state, title };
 }
 ```

## Basic usage

* Store setup
* Middleware
* Connected Component \(aka Container Component\)
* Reducers
* Dispatchers

## Peer Dependencies

## Installation

```
npm i --save @zazapeta/rx-react-store
```



