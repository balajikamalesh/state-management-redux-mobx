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
- Mutating Array and objects is also impure. This can be done by using "Object.Assign" or the Spread operator to create new objects or Arrays. 
We have to always return new objects to tell React or any other framework to re-render the DOM.

```js
let original = { a: 1, b: 2 };
let extension = { c: 3 };
let combined = {...original, ...extension}

let array = [1, 2, 3, 4];
let newArray = [0, ...array, 5];
```
