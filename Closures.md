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

Q2 Write a function that would allow you to do this

```js
var addSix = createBase();
addSix(10);
addSix(21);