# State Management using Redux and MobX

## Prerequisits
- Pure vs Impure Functions

```js
//pure
function add(a, b) {
    return a + b;
}

//impure
const b = 3;
function add(b){
    return a + b;
}

//impure
function POST(a){
    Api.POST(b)......
}
```
- Mutating Array and objects is also impure. This can be Avoided by using "Object.Assign" or the Spread operator to create new objects or Arrays. 
We have to always return new objects to tell React or any other framework to re-render the DOM.

```js
let original = { a: 1, b: 2 };
let extension = { c: 3 };
let combined = {...original, ...extension}

let array = [1, 2, 3, 4];
let newArray = [0, ...array, 5];
```

## Redux
- Installation - "npm i redux"
- The whole state tree of the application is kept in one store (A plain simple javascript object)
- Since it is a plain jsvascript, we can convert it to string and store in the database and then use this to hydrate the entire application to previous state. 
This will be espicially usefull while dealing with customer-found defect.
- Instead of directly modifying the application state, we dispatch an "Action". A "Reducer" takes this "Action" and produces a new application State.
- It has only 5 functions
- - applyMiddleware()
- - bindActionCreators()
- - combineReducers()
- - compose() ( for function composition )
- - createStor()

```js
const {
    compose,
    createStore,
    combineReducers,
    bindActionCreators,
    applyMiddleware
} = Redux;
```

### compose()
This is a functional programming utility, and is included in Redux as a convenience. You might want to use it to apply several store enhancers in a row.
```js
const add1 = (num) => num + 1;
const add2 = (num) => num + 2;
const add3 = (num) => num + 3;
const add6 = compose(add1, add2, add3);

console.log(add6(3)); //9

import { createStore, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
import DevTools from './containers/DevTools'
import reducer from '../reducers'

const store = createStore(
  reducer,
  compose(applyMiddleware(thunk), DevTools.instrument())
)
```

### createStore()
Creates a Redux store that holds the complete state tree of your app. There should only be a single store in your app.
```js
const reducer = (state = { value: 1 }, action) => {
  if(action.type === 'VALUE_ADD') {
    return { ...state, value: state.value + action.payload };
  } 
  return { ...state };
}

const store = createStore(reducer);

store.dispatch({
    type: 'VALUE_ADD',
    payload: 2
})
```

### combineReducers()
The combineReducers helper function turns an object whose values are different reducing functions into a single reducing function you can pass to createStore
```js
//todo.js
export default function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}
//counter.js
export default function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
//index.js
import { combineReducers } from 'redux'
import todos from './todos'
import counter from './counter'

export default combineReducers({
  todos,
  counter
})
//App.js
import { createStore } from 'redux'
import reducer from './reducers/index'

const store = createStore(reducer)
```

### applyMiddleware()
Middleware is the suggested way to extend Redux with custom functionality.
```js
const store = createStore(reducer, applyMiddleware(logger))
```

## React-Redux
- Installation - "npm i react-redux"
- For new "create-react-app" applications - "npx create-react-app my-app --template redux"
- It lets your React components read data from a Redux store, and dispatch actions to the store to update state
- 2 important methods
- - connect()
- - Provider
```js
import { createStore } from 'redux';
import { connect, Provider } from 'react-redux';
```
