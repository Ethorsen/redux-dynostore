# @redux-dynostore/redux-saga

[![npm version](https://img.shields.io/npm/v/@redux-dynostore/redux-saga.svg?style=flat-square)](https://www.npmjs.com/package/redux-dynostore-redux-saga)
[![npm downloads](https://img.shields.io/npm/dm/@redux-dynostore/redux-saga.svg?style=flat-square)](https://www.npmjs.com/package/@redux-dynostore/redux-saga)
[![License: BSD-3-Clause](https://img.shields.io/npm/l/@redux-dynostore/redux-saga.svg?style=flat-square)](/LICENSE.md)

Enhancer and handler for dynamically running sagas.

## Usage

Include dynamic enhancer with [`redux-dynostore`](/packages/redux-dynostore-core):

```javascript
import dynostore from '@redux-dynostore/core'
import { dynamicSagas } from '@redux-dynostore/redux-saga'

const sagaMiddlware = createSagaMiddleware()

const store = createStore(reducer, compose(
  applyMiddleware(sagaMiddlware),
  dynostore(dynamicSagas(sagaMiddlware))
)
```

Run the saga when the [`dynamic`](/packages/redux-dynostore-react-redux) component mounts:

```javascript
import dynamic from '@redux-dynostore/react-redux'
import { runSaga } from '@redux-dynostore/redux-saga'

export default dynamic('identifier', runSaga(mySaga))(MyComponent)
```

### Manually running sagas

If you aren't using react, or want to run a saga outside of the component lifecycle, the store now has an `runSagas` function that can be used to run sagas if you only have access to the store:

```javascript
store.runSagas({ dynamicSaga })
```

Multiple sagas can be run as well:

```javascript
store.runSagas({ dynamicSaga1, dynamicSaga2 })
```

Sagas can also be provided in a nested structure, but this only provides simplified identifier creation:

```javascript
store.attachReducers({ 'some/path/to': dynamicSaga })
```

```javascript
store.attachReducers({
  some: {
    path: {
      to: {
        dynamicSaga
      }
    }
  }
})
```

Both the above examples are equivalent to calling:

```javascript
store.attachReducers({ 'some.path.to': dynamicSaga })
```