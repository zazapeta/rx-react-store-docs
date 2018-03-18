# Reducer

{% method %}

Reducers are ** pure functions ** that receive the current state of the store and arguments and return a new state of the store. 

## Pure Functions
A **pure function ** is a function which:
 * Given the same input, will always return the same output.
 * Produces no side effects.

 [Eric Elliot](https://twitter.com/_ericelliott) describes (in this [article](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)) very well  what are the payoff using pure function.

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

