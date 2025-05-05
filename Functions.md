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