```js
async dispatch(reducer = (state) => state, ...rest) {
    let commonArgs = [this.state, reducer, ...rest];
    await promiseAllMap(BeforeGlobalParalel, ...commonArgs);
    await promiseAllMap(this.BeforeLocalParalel, ...commonArgs);
    await promiseSeqMap(BeforeGlobalSequential, ...commonArgs);
    await promiseSeqMap(this.BeforeLocalSequential, ...commonArgs);
    this._subject.next(reducer(this.state, ...rest));
    await promiseAllMap(AfterGlobalParalel, ...commonArgs);
    await promiseAllMap(this.AfterLocalParalel, ...commonArgs);
    await promiseSeqMap(AfterGlobalSequential, ...commonArgs);
    await promiseSeqMap(this.AfterLocalSequential, ...commonArgs);
  }
```