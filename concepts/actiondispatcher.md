# Dispatcher

Dispatcher are  **async functions**  that receive a [reducer](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/concepts/reducer.md) and, in addition some arguments that will be passed to the reducer. This the one way to modify the store. Like `react.setState` `dispatch` is async. Use `await` keyword to make things after a dispatch. There is 2 ways to `dispatch`:

* Using the [`dispatch`](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/methods.md#dispatch) method direclty
* Calling a dispatcher created by [`createDispatcher`](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/methods.md#createDispatcher) or [`createDispatchers`](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/methods.md#createDispatchers)

Dispatching will trigger [middlewares](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/middlewares-lifecycle.md) and will rerender all  **connected components** that are wrapped by the [`connect`](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/methods.md#connect) HOC of a same RxStore instance \(think singleton\).

## _async_ `dispatch(reducer)`

Using `dispatch` method directly. Be aware with this method. It can generate a lot of boilerplate code. To reduce it, see [`createDispatchers`](https://github.com/zazapeta/rx-react-store-docs/tree/a9d88d16c10c8bf43148794a0fb8dd0b5d981655/concepts/methods.md#createDispatchers)

### Example

_todo.reducers.js_

```javascript
import {createTodo} from './todo.utils.js';

export function addTodo(state, todo){
 return {
  ...state, // We don't mutate the state. We create a copy.
  todos: state.todos.concat(createTodo(todo))
 }
}
```

_todo.addInput.container.jsx_

```javascript
import { addTodo } from './todo.reducers.js';
import todoStore from './todo.store.js';

export default function AddInput(){
 return <input type='text' onBlur={(e) => todoStore.dispatch(addTodo, e.target.value)};
}
```

## `createDispatcher(reducer, ...rest)`

`createDispatcher` is more like a built-in helper to reduce repetitive code around `dispatch`;

### Example

_todo.reducers.js_

```javascript
import {createTodo} from './todo.utils.js';

export function addTodo(state, todo){
 return {
  ...state, // We don't mutate the state. We create a copy.
  todos: state.todos.concat(createTodo(todo))
 }
}
```

_todo.dispatchers.js_

```javascript
import { addTodo } from './todo.reducers.js';
import todoStore from './todo.store.js';

export const handleTodoAdd = todoStore.createDispatcher(addTodo); // equivalent at (todo) =>  todoStore.dispatch(addTodo, todo);
```

_todo.addInput.container.jsx_

```javascript
import { handleAddTodo } from './todo.dispatchers.js';

export default function AddInput(){
 return <input type='text' onBlur={(e) => handleAddTodo(e.target.value)};
}
```

## `createDispatchers(mapReducers:Object<key,reducer>):Object<key, dispatcher>`

`createDispatchers` is more like a built-in helper to reduce repetitive code around `createDispatcher`.

### Example

_todo.reducers.js_

```javascript
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

```javascript
import todoStore from './todo.store.js';
import * as todoReducers from './todo.reducers.js';

const { addTodo: handleAddTodo } = todoStore.createDispatchers(todoReducers);

export default function AddInput(){
 return <input type='text' onBlur={(e) => handleAddTodo(e.target.value)};
}
```

