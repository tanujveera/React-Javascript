# Functions

### Function declaration

`Hoisting`: Declarations are hoisted with their body. You can call square() before the line where it’s written.

`Name`: Always has a name (square), which shows up in stack traces and inside the function as square.name.
```js
// Function definition or function statement
function square(num) {
    return num * num;
}
console.log(square(5)) // 25

// We can access the function definition as it is hoisted to the top
console.log(square(5)) // 25
function square(num) {
    return num * num;
}
```

### Function Expression
A function without any name (anonymous function) assigned to a variable

`Not hoisted`: Only the variable name (multiply) is hoisted (as undefined), but the function itself isn’t created until that line runs.

- Can be anonymous or named:
- Anonymous: `function(x,y){…}`
- Named: `function div(x,y){…}`, helpful in debugging/recursion.

```js
// Anonymous function expression
const multiply = function(x, y) {
  return x * y;
};

// Named function expression (name is local to the function)
const divide = function div(x, y) {
  return x / y;
};

console.log(multiply);     // undefined
console.log(multiply(4,2)); // TypeError: multiply is not a function

const multiply = function(x, y) {
  return x * y;
};
```

### First Class Functions

`First-class` means a language treats functions like any other value. In JavaScript, you can:

- Assign a function to a variable
- Pass a function as an argument to another function
- Return a function from another function
- Store functions in arrays or objects

```js
// Assign to a variable
const sayHi = function() {
  console.log("Hi!");
};

// Pass as an argument
function repeat(fn, times) {
  for (let i = 0; i < times; i++) fn();
}
repeat(sayHi, 3);

// Return from a function
function makeGreeter(name) {
  return function() {
    console.log(`Hello, ${name}!`);
  };
}
const greetUma = makeGreeter("Hello");
greetHello();  // “Hello, Hello!”
```

## IIFE - Immediately Invoked Function Expression

A function which invokes itself is a IIFE
```js
// Ex 1
(function x(n){
    console.log(n*n);
})(5) // 25

// Ex 2
// inner function will search for x in its scope
// if the x is not found it searches in its parent scope
(function (x) {
    return (function (y){
        console.log(x) // 1
    })(2)
})(1)
```

# Function Scope

- `var` is function-scoped, not block-scoped.
- By the time any of the `setTimeout` callbacks are executed, the loop has already completed, and i has been incremented to 5.
- All the scheduled functions share the same `i` variable, which now holds the value 5.
- So when `setTimeout` fires (after 0ms, 1000ms, ..., 4000ms), they all reference the same i, which is now 5.

```js
for(var i =0; i<5;i++){
    setTimeout(function () {
        console.log(i);
    },i*1000);
}
/* O/p
5
5
5
5
5
*/
```
---
- `let` is block-scoped.
- Each iteration of the loop creates a new binding of `i`, scoped to that iteration.
- So, when the `setTimeout` executes, it remembers the correct `i` for that iteration via closure over a fresh variable.
- This is one of the key reasons why `let` is preferred in modern JavaScript over `var` for block-scoped behavior.

```js
for(let i =0; i<5;i++){
    setTimeout(function () {
        console.log(i);
    },i*1000);
}
/* O/p
0
1
2
3
4
*/
```

## Using IIFE
- If you want to preserve the current value of i, you can create a new scope using an `IIFE` (Immediately Invoked Function Expression).
- Creates a new execution context for each value of i.
- Passes i as j, which is locally scoped to that IIFE call.
- The setTimeout callback closes over j, not i.
- This closure ensures each timeout logs the correct value of j.

```js
for (var i = 0; i < 5; i++) {
    (function (j) {
        setTimeout(function () {
            console.log(j);
        }, j * 1000);
    })(i);
}
```

## Using Closure

```js
for(var i =0; i<3; i++){
    function inner(i){
        setTimeout(function log(){
        console.log(i);
        }, i*1000);
    }
    inner(i)
}
```

# Function Hoisting

- Functions are hoisted differently in JS.
- If you see the source tab in browser, you can find the functionName with whole function definition but variable `x` is undefined. 

```js
functionName() // o/p: Function Name
console.log(x); // o/p: undefined

function functionName() {
    console.log("Function Name");
}

var x =9;
```

- Even though this is function scope, `x` is hoisted in local scope with `undefined`
```js
functionName()

function functionName() {
    console.log(x); // undefined
    var x =9;
    console.log("Function Name"); // Function Name
}
```

- This code returns `undefined` since the `x = 9` in function will shadow the global scoped `x = 8` and the printing the `x` even before the variable is declared.
```js
var x = 8;

function functionName() {
    console.log(x);
    var x =9;
}

functionName()
```

# Params vs Arguments

```js
function square(num){ // Parameters or Params
  console.log(num * num);
}

square(5); // Arguments
```

# Spread / Rest Operator
**Rest Operator**
 - Rest - Gather values into new array
```js
// REST operator
function multiply (...nums) { // Rest Operator
    console.log(nums[0] * nums[1])
}
// Function parameters
function sum(...numbers) {
  console.log(numbers); // an array
  return numbers.reduce((acc, curr) => acc + curr, 0);
}
console.log(sum(1, 2, 3, 4)); // 10

// REST - Destructuring
const [first, ...rest] = [10, 20, 30, 40];
console.log(first); // 10
console.log(rest);  // [20, 30, 40]

// Object - Destructuring
const user = { name: "Tanuj", age: 25, city: "Delhi" };
const { name, ...others } = user;

console.log(name);   // "Tanuj"
console.log(others); // { age: 25, city: "Delhi" }
```

**Spread Operator**
 - Spread - Spreads values (copy to existing or new array)
```js
var arr = [5,6]
multiply(...arr) // Spread Operator

// merging 2 arrays
const a = [1, 2];
const b = [3, 4];
const combined = [...a, ...b];

console.log(combined); // [1, 2, 3, 4]

// extending values
const user = { name: "Tanuj" };
const updatedUser = { ...user, age: 25 };

console.log(updatedUser); // { name: "Tanuj", age: 25 }

```

- Rest Operator must be the last formal parameter, so this code will throw error

```js
const fn = (a,...numbers,x,y) => {
    console.log(x,y)
}

fn(2,3,4,3,5,9)
```

- Here `a = 2` `x=3` `y=4` and remaining are collected in `...numbers` rest operator
```js
const fn = (a,x,y,...numbers) => {
    console.log(x,y) // 3, 4
    console.log(numbers) // [0,5,9]
}

fn(2,3,4,0,5,9)
```

# Callback Function

- A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

```js
function greet(name, callback) {
    console.log("Hello, " + name + "!");
    callback();
}

function sayGoodbye() {
    console.log("Goodbye!");
}

greet("Alice", sayGoodbye);

// Output
// Hello, Alice!
// Goodbye!
```

# Arrow Functions

```js
// 1 - Syntax
function square(num) {
    return num * num;
}

const squareFn = (num) => {
    return num * num;
}

// 2 - Implicit 'return' keyword
const squareArr = (num) => num * num;

// 3 - arguments
// You can use arguments keyword to get the function arguments even if params are not defined.
function fn (){
    console.log(arguments); // [Arguments] { '0': 1, '1': 3, '2': 2 }
}

fn(1,3,2)
// ---
const fnArr = () => {
    console.log(arguments); // ReferenceError
}

fnArr(1,3,2)

// 4 - 'this' keyword
/* Here, rc1 is a arrow function and this keyword will check the global scope
But rc2 function will check local scope which is in user object. 
*/
let user = {
    username : 'tanuj veera',
    rc1: ()=> console.log("rc1 username "+ this.username),
    rc2 (){console.log("rc1 username "+ this.username)},
}

user.rc1() // undefined
user.rc2() // tanuj veera
```

## Function Currying
[Function Currying link](https://javascript.info/currying-partials)