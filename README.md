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

```js
let a;
let a;
```