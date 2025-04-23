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

