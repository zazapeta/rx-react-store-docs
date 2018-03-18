# Dispatcher


Dispatcher are ** async functions ** that receive a [reducer](concepts/reducer.md) and, in addition some arguments that will be passed to the reducer. This the one way to modify the store.
There is 2 ways to `dispatch`:
 * Using the [`dispatch`](methods.md#dispatch) method direclty
 * Calling a dispatcher created by [`createDispatcher`](methods.md#createDispatcher) and [`createDispatchers`](methods.md#createDispatchers)
 
Dispatching will trigger middlewares and will rerender all ** connected components** that are wrapped by the [`connect`](methods.md#connect) HOC of a same RxStore instance.

{% method %}

Dispatcher are ** async functions ** that receive a [reducer](concepts/reducer.md) and, in addition some arguments that will be passed to the reducer.
## Dispatching a reducer

To `dispatch` a reducer 
{% common %}
###Example

_todo.reducers.js_

```js
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
```js
export function addItem(state, list, item){
 return {
  ...state,
  [list]: state[list].concat(item)
 }
}

export function removeItem(state, list, item, id){
 return {
  ...state,
  [list]: state[list].filter((i) => id(i) !== id(item))
 }
}

```

 and rewrigth 
 _todo.reducers.js_
 
 ```js
import { addItem, removeItem } from './common.reducers.js';

export const addTodo = (state, todo) => addItem(state, 'todos', todo);

export const removeItem = (state, todo) => removeItem(state, 'todos', todo, (todo) => todo.id);
```

{% endmethod %}

