# Reducer

Reducers are  **pure functions**  that receive the current state of the store and arguments and return a new state of the store.

## Pure Functions

A **pure function**  is a function which:

* Given the same input, will always return the same output.
* Produces no side effects.

[Eric Elliot](https://twitter.com/_ericelliott) describes \(in this [article](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)\) very well what is the payoff of pure functions.

In this example the classic switch case of [redux reducer](https://redux.js.org/basics/reducers#handling-more-actions) disapeared. There is no more combine Reducer.

With `RxStore` there is no more `switch case` because we dispatch direclty a reducer to affect the state.

`Reducers` are destined to be dispatched thought the `store.dispatch` method. To do so, we can create [Dispatchers](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/concepts/actiondispatcher.md).

### Example

_todo.reducers.js_

```javascript
export function addTodo(state, todo){
 return {
  ...state, // We don't mutate the state. We create a copy.
  todos: state.todos.concat(todo)
 }
}

export function removeTodo(state, todo){
 return {
  ...state, // again. 
  todos: state.todos.filter((t) => t.id !== todo.id))
 }
}
```

but we can imagine a more generic/functional way to split reducers like

_common.reducers.js_

```javascript
export function addItem(state, list, item){
 return {
  ...state,
 }
}

export function removeItem(state, list, item, id){
 return {
  ...state,
 }
}
```

and rewrigth _todo.reducers.js_

```javascript
import { addItem, removeItem } from './common.reducers.js';

export const addTodo = (state, todo) => addItem(state, 'todos', todo);

export const removeItem = (state, todo) => removeItem(state, 'todos', todo, (todo) => todo.id);
```

