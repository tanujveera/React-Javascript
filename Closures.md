# Closure

- A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). 
- In other words, a closure gives a function access to its outer scope. 
- In JavaScript, closures are created every time a function is created, at function creation time.

```js
var name = "tanuj";

// global scope
function localFn (){
    // local 
    console.log(name)
}

localFn()
```

The below code gives ReferenceError : name is not defined

```js
// global scope
function localFn (){
    // local 
    var name = "tanuj";
}
console.log(name)

localFn()
```

Closure with Function currying

```js
function makeFun () {
  var name = "tanuj";
  function displayName (num) {
    console.log(name,num) // tanuj 5
  }
  return displayName;
}

makeFun()(5)
```

## Closure Scope Chain
 - Local Scope (Own scope)
 - Outer Functions Scope
 - Global Scope

 ```js
 var username = "veera" // global scope
 function makeFun () {
  var name = "tanuj"; // outer function scope
  function displayName (num) {
    var d = 6; // local scope
    console.log(name,num,username,d) // tanuj 5 veera 6
  }
  return displayName;
}

makeFun()(5)
```

## Automatic Semicolon Insertion (ASI)
 - Here there is no semicolon for `let count = 0` 

```js
let count = 0 // no semicolon ;
// gives TypeError: 0 is not a function
(function printCount () {
  if (count === 0) {
    let count = 1;
    console.log(count);
  }
  console.log(count);
})();

// JS Engine will consider it in this way
let count = 0(function printCount () {...}) // TypeError: 0 is not a function

let count = 0; // no semicolon ;

(function printCount () {
  if (count === 0) {
    let count = 1;
    console.log(count); // 1
  }
  console.log(count); // 0
})();
```

## Interview Questions

Q1 Closure example for local and lexical scope using IIFE

```js
let count = 0;

(function printCount () {
  if(count === 0){
    let count = 1
    console.log(count) // 1
  }
  console.log(count) // 0
})()
```
---
Q2 Write a function that would allow you to do this (Closure)

```js
function createBase(num){
  return function (innerNum){
    console.log(num + innerNum);
  }
}

var addSix = createBase(6);
addSix(10); // 16
addSix(21); // 27
```
---
Q3 Time Optimization

```js
function find() {
  let a = [];
  for(let i = 0; i< 1000000; i++){
    a[i]= i*i;
  }
  return function (index) {
    console.log(a[index]) 
  }
}

const closure = find();

console.time("6")
closure(2) // 4
console.timeEnd("6") // 6: 6.653ms

console.time("50")
closure(50) // 2500
console.timeEnd("50") // 50: 0.128ms

console.time("6")
closure(600) // 360000
console.timeEnd("6") // 0.05ms
```
---
Q4 Block Scope and setTimeout
 - `var` is function scoped not block scope
 - `let` is block scoped.
```js
// using var
function a () {
  for(var i =0; i<3; i++){
    setTimeout(function log(){
      console.log(i); // 3 3 3
    }, i*1000);
  }
}

a();
// using let
function a () {
  for(let i =0; i<3; i++){
    setTimeout(function log(){
      console.log(i); // 0 1 2
    }, i*1000);
  }
}

a();
```
---
Q5 How would you use a closure to create a private counter?

```js
function counter() {
  var _counter = 0; // put _ when using private variables, just a convention

  function add(increment){
    _counter += increment;
  }
  
  function retrive(){
    return "Counter = "+_counter;
  }
  
  return {
    add,
    retrive
  }
}

const c = counter();
c.add(5)
c.add(10)
console.log(c.retrive()) // Counter = 15
```
---
Q6 What is **Module Pattern**?

- The Module Pattern is a design pattern in JavaScript used to create private and public encapsulation — essentially allowing data hiding and modularization of code using closures and Immediately Invoked Function Expressions (IIFEs).

- This is an Immediately Invoked Function Expression (IIFE):

  - (function () { ... })() runs immediately, and its return value is assigned to Module.

- Inside it:

  - privateMethod() is a private function, scoped only within the IIFE.
- Why Use the Module Pattern?
  - **Encapsulation**: Keep internal logic (private methods/variables) hidden.

  - **Avoid Global Scope Pollution**: Keeps variables local to the module.

  - **Organized Code Structure**: Public API clearly defined via returned object.

```js
var Module = (function (){
  function privateMethod(){
    // some code
    console.log("private")
  }
  return {
    publicMethod: function () {
      // can call privateMethod();
      console.log("Public")
    },
  }
})();

Module.publicMethod(); // Public
Module.privateMethod(); // TypeError: Module.privateMethod is not a function

// if private method is added in that public
var Module = (function (){
  function privateMethod(){
    // some code
    console.log("private")
  }
  return {
    publicMethod: function () {
      // can call privateMethod();
      privateMethod();
      console.log("Public")
    },
  }
})();

Module.publicMethod(); // private Public
Module.privateMethod(); // // TypeError: Module.privateMethod is not a function
```
---
Q7 Make this run only once

```js
// this code gives the response multiple times
let name;
function intoFunction (){
  name = "Tanuj Veera"
  console.log(name);
}

intoFunction() // Tanuj Veera
intoFunction() // Tanuj Veera
intoFunction() // Tanuj Veera

//---------------------------------------------------
// Now with a local variable called we can track if its called

let name;
function intoFunction (){
  let called = 0;
  return function () {
    if(called > 0) {
      console.log("Already Called the function")
    } else {
      name = "Tanuj Veera"
      console.log(name);
      called++;
    }
  }
}
let resFun = intoFunction()
resFun() // Tanuj Veera
resFun() // Already Called the function
resFun() // Already Called the function
```
---
Q8 Make this run only once polyfill (Once Utility function)

```js
function once(func,context) {
  let ran;
  return function () {
    if(func){
      ran = func.apply(context || this, arguments);
      func = null;
    }
    return ran;
  }
}

const hello = once(()=>console.log("hello"));

hello();
hello();
hello();
```

**Internal Working**
- `ran` is a local variable stored in the closure — it holds the return value of the original function.
- `func = null` ensures func cannot be called again — we remove the reference after first call.
- The returned function checks if func exists:
- If yes, it calls it and sets `func = null`.
- If no, it just returns the saved result.

**apply()** function is JS
- `func.apply(context, argsArray)` is a way to call a function with a specific this context and an array-like object of arguments.
```js
ran = func.apply(context || this, arguments);
```
- To forward all arguments given to the returned function, to the original func.
- To allow explicit binding of this using the optional context parameter.
- If no context is passed, it defaults to the current this.


**What is `this` in this case?**

In the returned function:
```js
return function () {
  ...
}
```

If you call it like:
```js
const hello = once(fn, obj);
hello();
```
- Inside the returned function, `this` depends on how it's called.
- But the code prefers `context || this,` so if you pass a `context`, that wins.

**Example** with **context** and **apply()**

```js
function once(func, context) {
  let ran;
  return function () {
    if (func) {
      ran = func.apply(context || this, arguments);
      func = null;
    }
    return ran;
  };
}

const person = {
  name: "Tanuj",
  greet() {
    console.log("Hello, " + this.name);
  },
};

const greetOnce = once(person.greet, person);

greetOnce(); // "Hello, Tanuj"
greetOnce(); // does not run
```
- We want `this` inside `greet()` to refer to `person`, not global or undefined.
- So `apply(context, ...)` ensures the right `this`.

**Optimized code for `Once` utility function**

```js
function once(fn, context) {
  let ran, done = false;
  return function (...args) {
    if (!done) {
      ran  = fn.apply(context ?? this, args);
      done = true;
    }
    return ran;
  };
}
```
---
## `??` **Nullish Coalescing Operator**

- `??` is the `Nullish Coalescing Operator` in JavaScript.

```js
a ?? b
```
- Return `a` if it's not `null` or `undefined`, otherwise return `b`.

How it's different from `||`:
- `||` checks if the left-hand value is falsy.
- If it’s falsy (like `false, 0, NaN, '', null, undefined`), it returns the right-hand value.
```js
const a = 0; // 0 or ''

console.log(a || 42);  // 42 (0 is falsy)
console.log(a ?? 42);  // 0  (0 is *not* null/undefined)

const b = '';
console.log(b || "tanuj"); // tanuj
console.log(b ?? "tanuj"); // "" (empty string)
```
---
Q9 Memoize Polyfill

```js
function myMemoize(fn,context) {
  const res = {};
  return function (...args) {
    var argsCache = JSON.stringify(args);
    if(! res[argsCache]){
      res[argsCache] = fn.call(context ?? this,...args);
    }
    return res[argsCache]
  }
}

const clumsyProduct = (num1,num2) => {
  for(let i=0;i<=1000000;i++) {}
  return num1 * num2;
}

const memoizedClumsyProduct = myMemoize(clumsyProduct);

console.time("First call");
console.log(memoizedClumsyProduct(6823,682348)) // 4655660404
console.timeEnd("First call") // First call: 9.829ms

console.time("First call");
console.log(memoizedClumsyProduct(6823,682348)) // 4655660404
console.timeEnd("First call") // First call: 0.08ms
```

