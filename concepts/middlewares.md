## Middleware 
Middleware is a function that will be executed in a specific moment, for a specific scope, for a specific mode.

We have 2 moments of execution : 
 * Before (will be executed before the `dispatch`)
 * After (will be executed after the `dispatch`

We have 2 scopes of execution : 
 * Local (will be executed only for the store that `dispatch`)
 * Global (will be executed for each `dispatch` - whathever the store is dispatching)

We have 2 modes of execution :
 * Parallel (will be executed in parallel mode - think `Promise.all`)
 * Sequential (will be executed in first-in-first-out (FIFO) mode - think `for in`)
 
So we ends-up with this liste of middlewares (executed in this order) :  
 * BeforeGlobalParallel
 * BeforeLocalParallel
 * BeforeGlobalSequential
 * BeforeLocalSequential

 <span style="color:red; border:1px solid red; margin:4px; border-radius:5px;"> <strong>DISPATCH</strong> </span>
 * AfterGlobalParallel
 * AfterLocalParallel
 * AfterGlobalSequential
 * AfterLocalSequential

  
<span style="border:1px solid black; margin:4px; font-size:18px;"><strong>Middlewares list are instances of <code>Map</code>.</strong></span>

## Example 
```js
import RxStore from '@zazapeta/rx-react-store';

const initialState = {
  title: 'Super App',
  version: 1,
};

const ns = 'App';

let appStore = new RxStore({ ns, initialState });
appStore.BeforeGlobalParallel.set('Perf', (state, action) =>
  console.time(`${action.name}`)
);

appStore.AfterGlobalParallel.set('Perf', (state, action) =>
  console.timeEnd(`${action.name}`)
);

appStore.AfterGlobalParallel.set('InfoLogger', (state, action) =>
  console.info(`[${action.name}] STATE:`, state),
);

export default appStore;
```


