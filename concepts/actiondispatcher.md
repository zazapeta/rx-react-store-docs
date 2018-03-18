# Dispatcher


Dispatcher are ** async functions ** that receive a [reducer](concepts/reducer.md) and, in addition some arguments that will be passed to the reducer. This the one way to modify the store.
There is 2 ways to `dispatch`:
 * Using the [`dispatch`](methods.md#dispatch) method direclty
 * Calling a dispatcher created by [`createDispatcher`](methods.md#createDispatcher) or [`createDispatchers`](methods.md#createDispatchers)
 
Dispatching will trigger [middlewares](middlewares-lifecycle.md) and will rerender all ** connected components** that are wrapped by the [`connect`](methods.md#connect) HOC of a same RxStore instance (think singleton).

{% method %}

## `dispatch(reducer)`

Using `dispatch` method directly. Be aware with this method. It can generate a lot of boilerplate code. To reduce it, see [`createDispatchers`](methods.md#createDispatchers)


{% common %}
###Example

_todo.reducers.js_

```js
import {createTodo} from './todo.utils.js';

export function addTodo(state, todo){
 return {
  ...state, // We don't mutate the state. We create a copy.
  todos: state.todos.concat(createTodo(todo))
 }
}

```

_todo.addInput.container.jsx_
```js
import { addTodo } from './todo.reducers.js';
import todoStore from './todo.store.js';

export default function AddInput(){
 return <input type='text' onBlur={(e) => todoStore.dispatch(addTodo, e.target.value)};
}
```

{% endmethod %}

{% method %}

## `createDispatcher(reducer, ...rest)`

`createDispatcher` is more like a built-in helper to reduce repetitive code around `dispatch`;


{% common %}
###Example
_todo.reducers.js_

```js
import {createTodo} from './todo.utils.js';

export function addTodo(state, todo){
 return {
  ...state, // We don't mutate the state. We create a copy.
  todos: state.todos.concat(createTodo(todo))
 }
}

```

_todo.dispatchers.js_
```js
import { addTodo } from './todo.reducers.js';
import todoStore from './todo.store.js';

export const handleTodoAdd = todoStore.createDispatcher(addTodo); // equivalent at (todo) =>  todoStore.dispatch(addTodo, todo);
```

_todo.addInput.container.jsx_
```js
import { handleAddTodo } from './todo.dispatchers.js';

export default function AddInput(){
 return <input type='text' onBlur={(e) => handleAddTodo(e.target.value)};
}
```

{% endmethod %}

{% method %}

## `createDispatchers(mapReducers:Object<key,reducer>):Object<key, dispatcher>`

`createDispatchers` is more like a built-in helper to reduce repetitive code around `createDispatcher`.


{% common %}
###Example
_todo.reducers.js_

```js
import {createTodo} from './todo.utils.js';

export function addTodo(state, todo){
 return {
  ...state, // We don't mutate the state. We create a copy.
  todos: state.todos.concat(createTodo(todo))
 }
}

export function ...

```

_todo.addInput.container.jsx_
```js
import todoStore from './todo.store.js';
import * as todoReducers from './todo.reducers.js';

const { addTodo: handleAddTodo } = todoStore.createDispatchers(todoReducers);

export default function AddInput(){
 return <input type='text' onBlur={(e) => handleAddTodo(e.target.value)};
}
```

{% endmethod %}








