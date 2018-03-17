#Methodes

{% method %}
## constructor {#constructor}


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
## dispatch {#dispatch}

My first method exposes how to print a message in JavaScript and Go.

{% common %}
Whatever language you are using, the result will be the same.

{% endmethod %}

---

{% method %}
## createDispatcher {#createDispatcher}

My first method exposes how to print a message in JavaScript and Go.

{% common %}
Whatever language you are using, the result will be the same.

{% endmethod %}

---

{% method %}
## createDispatchers {#createDispatchers}

My first method exposes how to print a message in JavaScript and Go.

{% common %}
Whatever language you are using, the result will be the same.

{% endmethod %}

---

{% method %}
## connect {#connect}

My first method exposes how to print a message in JavaScript and Go.

{% common %}
Whatever language you are using, the result will be the same.

{% endmethod %}





