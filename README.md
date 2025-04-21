# React-Javascript

## Scope
Certain region of program where a variable can be exist and can be recognized

1. Block Scope
2. Function Scope
3. Global Scope

```js
var a = 5
console.log(a); // 5
```

- `var` is functional scope.
- `let` `const` are blocked scope.
```js
{
    var a = 5;
    let b= 6;
    const c = 9;
}
console.log(a); // 5
console.log(b); // Uncaught ReferenceError: b is not defined
console.log(c); // Uncaught ReferenceError: c is not defined
```

## Shadowing
Shadowing of a variable - the same variable value is different blocked scopes then values differ.

```js
function test(){
    let a = "Hi";

    if(true){
        let a = "Hello";
        console.log(a); // Hello
    }
    console.log(a); // Hi
}
```

## Illegal Shadowing

```js
function test(){
    var a = "Hi";
    let b = "B value";

    if(true){
        let a = "Hello"; // This kind of variable assignment is fine from var to let but let to var causes Syntax error. 
        var b= "duplicate";
        console.log(a); // Hello
        // throws SyntaxError: Identifier 'b' is already been declared
        console.log(b); 
    }
}
```

## Re-declare the variables

We can redeclare the `var` but `let` & `const` can't be redeclared again
```js
var a;
var a;

let a;
let a; // Uncaught SyntaxError: Identifier 'a has already been declared
// below declaration is fine as it they are in different scopes.
let b;
{
    let b;
}
//------------
const a; // SyntaxError: Missing initializer in const declaration

//-------
const a = 9;
const a = 8; // Uncaught SyntaxError: Identifier 'a has already been declared

//--------
var e = 9;
e = 8; // works fine

let h = 7;
h = 4; //works fine

const c = 8;
c = 6; // TypeError: Assignment to constant variables
```

## Hoisting
During creation phase, all variable and function declarations are moved to the top of the code.

```js
console.log(a); // undefined
var a = 1;

console.log(b);
let b= 1; //ReferenceError: Cannot access 'b' before initialization

console.log(c);
const b= 1; //ReferenceError: Cannot access 'b' before initialization
```

## Temporal Dead Zone
The time between the initialization and declaration of `let` and `const` variables.

```js
console.log(a);
let a = 1;
var b = 2;
```
In Browsers, in Sources > source section there will be 2 Objects (`Global`,`Script`) `var` variables is found in `Global` and `let` variables are found in `Script` section. Both will be `undefined`.

We get `ReferenceError` when we access the `let` and `const` variables but internally they are hoisted in temporal dead zone and variables as undefined.

## map, filter and reduce

### map()
- map(): used to create a new array from existing one by applying a function to each element.
- map takes 3 arguments - each element (num), index (i), array itself (arr which is nums)
- map doesn't modify the existing array but creates a new one.
```js
const nums = [1,2,3,4]
const multiply = nums.map((num,i,arr)=>{
    return num*3;
})
console.log(nums); // [ 1, 2, 3, 4 ]
console.log(multiply); //[ 3, 6, 9, 12 ]
```

### filter()

- filter() takes each element and applies a conditional statement against it and if it passes the condition then only it is pushed into the array.

```js
const nums = [1,2,3,4]
const moreThenTwo = nums.filter((num,i,arr)=> {
    return num>2;
})
console.log(nums);
console.log(moreThenTwo);
```

