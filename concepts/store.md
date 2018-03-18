The Store is a simple Class with a very basic API to be used: 
 * dispatch(reducer, ...args)
 * connect
 
## Example

_app.store.js_
```js
import RxStore from '@zazapeta/rx-react-store';

const initialState = {
  title: 'Super App',
  version: 1,
};

const ns = 'App';

let appStore = new RxStore({ ns, initialState });

appStore.AfterGlobalParalel.set('InfoLogger', (state, action) =>
  console.info(`[${action.name}] STATE:`, state),
);

export default appStore;
```

_App.container.jsx_
```js
import appStore from './app.store.js';

function App({title}){
  return (
    <div>
      <h1>{title}</h1>
      <input 
        type='text'
        onChange={(e) => appStore.dispatch((state) => ({...state, title:e.target.value})}
      />
    </div>
  );
}

export 

```