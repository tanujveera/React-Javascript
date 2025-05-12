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
      console.log(i);
    }, i*1000);
  }
}

a();
```

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

Q6 What is Module Pattern?

```js

```