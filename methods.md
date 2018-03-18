#Methodes

{% method %}
## constructor({ns:String, intialState:Object}) {#constructor}


```js
  /**
   * Create an RxStore instance
   * @param {Object} opts
   * @param {String} opts.ns The namespace of the store. Debuggin purpose. Default: 'rxStore'
   * @param {Object} opts.initialState Initial state of the store. Default: {}
   * 
   * @returns {RxStore} A new RxStore instance
   */
   constructor({ ns = 'rxStore', initialState = {} } = {})
 ```

{% common %}
###Example

_todo.store.js_

```js
import RxStore from '@zazapeta/rx-react-store';

const ns = 'Todo';
const initialState = {
  todos: []
};

const todoStore = new RxStore({ ns, initialState });

export default todoStore;
```

{% endmethod %}

---

{% method %}
## _async_ dispatch(reducer:Function, ...rest) {#dispatch}

```js
  /**
   * Async Method
   * Dispatch a reducer thought the store. This the unique manner to modify the store.
   * @param {Function(Object,...rest)} action The reducer to be dispatched, that will modify the store.
   * Action got the state of the store as first arguments and 'rest' next.
   * @param {*} rest Rest of arguments passed to middlewares and to the action.
   */
   async dispatch(reducer = (state) => state, ...rest)
```
{% common %}

Define dispatchers in a dedicated file.
Just import `todoStore` and have access to the `dispatch` method to update all connected component.

_todo.dispatchers.js_
```js
import todoStore from './todo.store.js';

export async function handleRemoveTodo(todo){
  return todoStore.dispatch((state) => ({
    ...state, 
    todos: state.todos.filter((t) => t.id !== todo.id)
  }));
}
```
{% endmethod %}

---

{% method %}
## createDispatcher(reducer:Function):Function {#createDispatcher}

`RxStore` provide `createDispatcher` method **to avoid repetitive code**.


```js
  createDispatcher(reducer) {
    return (...args) => this.dispatch(reducer, ...args);
  }
```

{% common %}

Define `reducers` in a dedicated file.

_todo.reducers.js_
```js
export function addTodo(state, todo){
  return {
    ...state,
    todos: state.todos.concat(todo)
  };
}
```

_todo.container.jsx_
```js
import todoStore from './todo.store.js';
import { addTodo } from './todo.reducers.js';
let handleAddTodo = todoStore.createDispatcher(addTodo);

/* Equivalent to :
function handleTodo(todo){
  return todoStore.dispatch(addTodo, todo);
}
*/
...

onClick={() => handleAddTodo(todo)}
```

{% endmethod %}

---

{% method %}
## createDispatchers(reducersMap_:Object<key:String,reducer:Function>_)_:dispatchersMap:Object<key:String, dispatcher:Function>_ {#createDispatchers}

`RxStore` provide `createDispatchers` method **to avoid repetitive code**.

```js
  createDispatchers(reducers = {}) {
    let dispatchers = {};
    Object.keys(reducers).forEach(
      (reducer) =>
        (dispatchers[reducer] = this.createDispatcher(reducers[reducer])),
    );
    return dispatchers;
  }
```

{% common %}

Define `reducers` in a dedicated file.

_todo.reducers.js_
```js
export function addTodo(state, todo){
  return {
    ...state,
    todos: state.todos.concat(todo)
  };
}
```

_todo.container.jsx_
```js
import todoStore from './todo.store.js';
import * as reducers from './todo.reducers.js';
let dispatchers = todoStore.createDispatchers(reducers);

...

onClick={() => dispatchers.addTodo(todo)}

```

{% endmethod %}

---

{% method %}
## connect(mapStateToProps_:Function(state,props)->Object_)(BaseComponent_:Component_)_:Component_ {#connect}

`connect` is a High Order Component (react-redux.connect).
It's create a component that listen for changes comming from the given store.

>TIPS: To integrate RxStore with another view library, you may override `connect`


{% common %}
_todo.container.jsx_
```js
import todoStore from './todo.store.js';

function TodoContainer({todos}){
...
}

function mapStateToProps({ todos }, props) {
  // Work very well with 'reselect'.
  let _todos = todos.sort((t1, t2) => t1.id < t2.id);

  return { todos: _todos };
}

export default todoStore.connect(mapStateToProps)(TodoContainer);
```

{% endmethod %}





