# Middleware

## Middleware {#lifecycle}

Middleware is a function that will be executed in a specific moment, for a specific scope, for a specific mode.

We have 2 moments of execution :

* Before \(will be executed before the `dispatch`\)
* After \(will be executed after the `dispatch`

We have 2 scopes of execution :

* Local \(will be executed only for the store that `dispatch`\)
* Global \(will be executed for each `dispatch` - whathever the store is dispatching\)

We have 2 modes of execution :

* Parallel \(will be executed in parallel mode - think `Promise.all`\)
* Sequential \(will be executed in first-in-first-out \(FIFO\) mode - think `for in`\)

So we ends-up with this liste of middlewares \(executed in this order\) :

* BeforeGlobalParallel
* BeforeLocalParallel
* BeforeGlobalSequential
* BeforeLocalSequential

   **DISPATCH**

* AfterGlobalParallel
* AfterLocalParallel
* AfterGlobalSequential
* AfterLocalSequential

**Middlewares list are instances of `Map`**

## Example

```javascript
import RxStore from '@zazapeta/rx-react-store';

const initialState = {
  title: 'Super App',
  version: 1,
};

const ns = 'App';

let appStore = new RxStore({ ns, initialState });
appStore.BeforeGlobalParallel.set('Perf', (state, reducer) =>
  console.time(`${reducer.name}`)
);

appStore.AfterGlobalParallel.set('Perf', (state, reducer) =>
  console.timeEnd(`${reducer.name}`)
);

appStore.AfterGlobalParallel.set('InfoLogger', (state, reducer) =>
  console.info(`[${reducer.name}] STATE:`, state),
);

export default appStore;
```

