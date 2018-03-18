# Reducer

{% method %}

Reducers are ** pure functions ** that receive the current state of the store and arguments and return a new state of the store. 

## Pure Functions
A **pure function ** is a function which:
 * Given the same input, will always return the same output.
 * Produces no side effects.

 [Eric Elliot](https://twitter.com/_ericelliott) describes (in this [article](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)) very well  what are the payoff using pure function.
 
 In this example the classic switch case of [redux reducer](https://redux.js.org/basics/reducers#handling-more-actions) disapeared.
 
 With `RxStore` there is no more `switch case` because we dispatch direclty a reducer to affect the state.

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

{% endmethod %}

