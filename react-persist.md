# middleware
- 미들웨어란 운영체제와 응용 소프트웨어 중간에서 조정과 중개의 역할을 수행하는 소프트웨어를 말한다.
- 리덕스 미들웨어는 액션을 디스패치 했을 때 리듀서에서 이를 처리하기에 앞서 사전에 지정된 작업을 실행한다. 미들웨어는 액션과 리듀서 사이의 중간자라고 볼 수 있다.

(https://velog.io/@gyumin_2/React.js-redux-middleware%EB%A6%AC%EB%8D%95%EC%8A%A4-%EB%AF%B8%EB%93%A4%EC%9B%A8%EC%96%B4-redux-thunk-redux-saga)

# React Persist
Persist and rehydrate a redux store.

- redux의 데이터는 메모리에 저장되며, 브라우저를 새로고침하거나 종료하게 되면 그 데이터는 휘발된다. 만약, 휘발되지 않게 하고 싶다면, redux에 react-persist를 붙여서 사용하면 된다.
- redux-persist는 redux store에 저장된 데이터를 브라우저의 로컬스토리지에 추가적으로 저장하고 불러오면서 데이터를 휘발되지 않게 유지한다.

## Quick start (공식 문서)
```bash
yarn add react-persist
```

## 일반적인 사용 방법
- 기존 redux로 이루어진 스토어 상태에서, 'redux-persist' 패키지의 **persistReducer**와 **persistStore**를 추가로 설정.
- 모든 앱은 'merge'할 상태의 수준을 몇 단계로 할지 결정해야한다. 기본 레벨은 1.

```js
// configureStore.js

import { createStore } from 'redux'
import { persistStore, persistReducer } from 'redux-persist'
import storage from 'redux-persist/lib/storage' // defaults to localStorage for web

import rootReducer from './reducers'

const persistConfig = {
  key: 'root',
  storage,
}

const persistedReducer = persistReducer(persistConfig, rootReducer)

export default () => {
  let store = createStore(persistedReducer)
  let persistor = persistStore(store)
  return { store, persistor }
}
```

만약, 리액트를 사용한다면 루트 컴포넌트를 PersistGate로 감싸주어야 함.
이때, PersistGate는 UI가 지연로딩 되도록 돕는 컴포넌트. 즉, 로컬스토리지와 redux의 상태가 동기화가 이루어진 후에 ui를 그리게 한다.

![GPT's answer](./images/gpt's%20answer.png)

```js
import { PersistGate } from 'redux-persist/integration/react'

// ... normal setup, create store and persistor, import components etc.

const App = () => {
  return (
    <Provider store={store}>
      <PersistGate loading={null} persistor={persistor}>
        <RootComponent />
      </PersistGate>
    </Provider>
  );
};
```

