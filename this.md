# "This" Keyword

The keyword `this` refers to the object that is executing the current function.

But what "object" it refers to depends on how the function is called, not where it's defined.

### Global Context

In the global scope, this refers to the global object:
- In browsers: window
- In Node.js: global

```js
console.log(this); // In browsers: `window`
```

### Inside Regular Function

- Regular functions (non-methods) in non-strict mode → this = global object.
```js
function show() {
  console.log(this);
}

show(); // In browser, logs `window`
```

- In strict mode ('use strict'), this will be undefined.
```js
'use strict';
function show() {
  console.log(this); // undefined
}
show();
```

### Inside a Method (Object Function)

- When a function is called as a method of an object, this refers to the object.
```js
const person = {
  name: "Tanuj",
  greet() {
    console.log(this.name);
  }
};

person.greet(); // "Tanuj"
```

### Using `this` in Arrow Functions

- Arrow functions don’t have their own this.
- They inherit this from the surrounding scope.

```js
const person = {
  name: "Tanuj",
  greet: () => {
    console.log(this.name);
  }
};

person.greet(); // undefined
```

Example

- Here, this inside the arrow function is inherited from the outer Person function.
```js
function Person() {
  this.name = "Tanuj";
  setTimeout(() => {
    console.log(this.name); // Tanuj ✅
  }, 1000);
}
```

### Using `this` with Constructors (new)
- When a function is called with new, this points to the newly created object.

```js
function User(name) {
  this.name = name;
}

const u1 = new User("Tanuj");
console.log(u1.name); // "Tanuj"
```

### `this` in Classes
- In class methods, `this` refers to the instance (object created with `new`).
```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(this.name);
  }
}

const p = new Person("Tanuj");
p.greet(); // "Tanuj"
```

### Losing this (Common Mistake)

```js
const user = {
  name: "Tanuj",
  greet() {
    console.log(this.name);
  }
};

const greetFn = user.greet;
greetFn(); // undefined, because it's now a regular function
```
To fix this, bind it:
- bind(): Returns a new function with this bound permanently
```js
const boundGreet = user.greet.bind(user);
boundGreet(); // "Tanuj"
```

## Interview Questions

### Q1 What is the output? This keyword
- f`irstname: "Tanuj"` → this is a property on the user object.
- Inside the method `getname`, you define a local variable const `firstname = "Tanuj Veera"`.
- `return this.firstname;`
- You are NOT returning the local variable `firstname`.
- You are explicitly accessing `this.firstname`, which means:
> Get the `firstname` property of the object that called this function (`user`).
```js
this → user
this.firstname → "Tanuj"
```

```js
const user = {
  firstname : "Tanuj",
  getname() {
    const firstname = "Tanuj Veera";
    return this.firstname
  }
}

console.log(user.getname()) // Tanuj
```
---
### Q2 What is the result of accessing its ref?
- In JavaScript, the value of this depends on how the function is called — not where it's defined.
- When makeUser() is called, it's called as a regular function (not as a method on an object). So this inside makeUser() points to:
    - In the browser: the global object (`window`)
    - In strict mode ('use strict'): it would be `undefined

This gets returned:
```js
let user = {
  name: "John",
  ref: window // or global object
};
```
>When access ref, we are accessing the `window` object.
```js
console.log(user.ref.name);
```
Its actually accessing
```js
window.name
```
- By default in most browsers, is an empty string ("").
```js
console.log(user.ref.name); // ""
```

In some environments, if window.name is not explicitly set, it will be:
- `""` (empty string)
- or `undefined`

### Correct Approach (if you want ref to refer to the object itself):

```js
function makeUser() {
  return {
    name: "John",
    ref() {
      return this;
    },
  };
}

let user = makeUser();

console.log(user.ref().name); // "John"
```
---
### Q3 What is the output ?
- Here, `this.name` will print empty or undefined.
- You're passing just the function `user.logMessage` to `setTimeout`, without calling it as `user.logMessage()`.
- So now, `logMessage` gets detached from the `user` object.
- When `setTimeout` calls it, it calls it as a regular function, not as a method of `user`.
- `this` no longer refers to `user`.
- In non-strict mode, `this` becomes the global object (`window` in browsers).
- So `this.name` becomes `window.name`, which is usually an empty string (`""`), or `undefined`.
```js
const user = {
    name: "Piyush Agarwal",
    logMessage() {
        console.log(this.name); // "" or undefined
    }
}

setTimeout(user.logMessage,1000)
```
>How to fix the above code?

Way 1
- You can fix the above code using a function inside a setTimeout, and call that function.
- Inside `setTimeout`, you are calling `user.logMessage()` explicitly as a method of `user`.
- So, the value of `this` inside `logMessage()` will correctly refer to `user`.
```js
const user = {
    name: "Piyush Agarwal",
    logMessage() {
        console.log(this.name);
    }
}

setTimeout(function(){
    user.logMessage()
},1000)
``` 

Way 2
- Use .bind() to bind this

```js
setTimeout(user.logMessage.bind(user), 1000);
```

Way 3
- Use an arrow function
```js
setTimeout(() => user.logMessage(), 1000);
```
---
### Q4 What is the output? this in object function and arrow function

```js
const user = {
    name:"tanuj",
    greet (){
        return `hello, ${this.name}!`
    },
    farewell: () => {
        return `Goodbye ${this.name}!`
    }
}

console.log(user.greet()) // hello, tanuj!
console.log(user.farewell()) // Goodbye !
```
---
### Q5 Create a calculator object

```js
let calculator = {
    read() {
        this.a = prompt("a = ",0) // 5
        this.b = prompt("b = ",0) // 5
    },
    sum () {
        return this.a + this.b;
    },
    mul () {
        return this.a * this.b;
    }
}

calculator.read()
console.log(calculator.sum()); // 55
console.log(calculator.mul()); // 25
```
- `prompt()` will take input as `string`.
- Two ways to solve this issue.
 
Way 1
- `+` unary operator which converts the string to number.
```js
this.a = + prompt("a = ",0)
```

Way 2
```js
this.a = Number(prompt("a = ",0))
```

```js
let calculator = {
    read() {
        this.a = + prompt("a = ",0) // 5
        this.b = + prompt("b = ",0) // 5
    },
    sum () {
        return this.a + this.b;
    },
    mul () {
        return this.a * this.b;
    }
}

calculator.read()
console.log(calculator.sum()); // 10
console.log(calculator.mul()); // 25
```
---
### Q6 Tricky question on this

```js
var length = 4;
function callback () {
    console.log(this.length);
}
const object = {
    length: 5,
    method(fn) {
        fn();
    },
}
object.method(callback); // 4
```

If you use `let` or `const` then,
- `let` and `const` do not create properties on the global object.
- So `window.length` remains undefined or unchanged.
- `this.length` (i.e., window.length) in that case will not be 4. It will probably show:
    - `0` in Node.js (if no global length)
    - Or an existing` window.length` value in browsers (which may be something else, depending on the environment)
```js
let length = 4; // 0
const length = 4; // 0
```

Different Scenario where the function is taken as **arguments**

```js
var length = 4;
function callback() {
    console.log(this.length);
}
const object = {
    length: 5,
    method() {
        arguments[0](); // <- Call the first argument (callback)
    },
};
object.method(callback, 2, 3); // 3
```

- You're calling the callback function, but from inside object.method(), using arguments[0]().
- Here's the key point:
    - In **non-strict mode**, if a function is called using `arguments[index]()`, the value of `this` inside that function is the `arguments` object itself.
- The `arguments` object
    - Inside object.method(callback, 2, 3)
```js
arguments = [callback, 2, 3]
```
- `arguments[0]()` → `this` becomes the `arguments` object.
- Then inside `callback`, you are logging `this.length`.
```js
console.log(this.length); // this is arguments → arguments.length === 3
```

>**If you add 'use strict'; at the top, then this inside callback() becomes undefined, and accessing this.length will throw an error.**

---
### Q7 Implement calculator

```js
const calc = {
    total:0,
    add(a) {
        this.total += a;
        return this;
    },
    multiply(a){
        this.total *= a;
        return this;
    },
    subtract(a) {
        this.total -= a;
        return this;
    }
}
const result = calc.add(10).multiply(5).subtract(30).add(10);
console.log(result.total) // 30
```