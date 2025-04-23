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
During creation phase, all variable and function declarations are moved to the top of the scope before execution.

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

### reduce()

- It reduces will take elements of array and return a single value.

```js
const nums = [1,2,3,4]
// acc - accumulator is the previous value and initial value is 0 in this case as mentioned as 2nd parameter
// curr is the current value in array, initially it will be 1st element
// i - index
// arr - is the array
const sum = nums.reduce((acc, curr,i ,arr)=>{
    console.log("acc "+acc+" curr "+curr)
    return acc+curr;
},0)
console.log(nums);
console.log(sum);

// O/p:
acc 0 curr 1
acc 1 curr 2
acc 3 curr 3
acc 6 curr 4
[ 1, 2, 3, 4 ]
10
```

## Polyfill
A `polyfill` is a piece of code that enables the usage of new programming language or web platform features in outdated browsers or environments that do not support them.

### Polyfill for map()
```js
// Array.map((num, i , arr)=> { })
//Creating a polyfill
Array.prototype.myMap = function (cb) {
    let temp = [];
    for(let i = 0; i< this.length;i++){
        temp.push(cb(this[i],i,this));
    }
    return temp;
}

const nums = [1,2,3,4];
// using the new map prototype implementation
const multipleThree = nums.myMap((num,i,arr)=>{
    return num*3;
})

console.log(multipleThree) // [3,6,9,12]
```

### Polyfill for filter()

```js
// Array.Filter((num, i , arr)=> { })
//Creating a polyfill
Array.prototype.myFilter = function (cb) {
    let temp = [];
    for(let i = 0; i< this.length;i++){
        if(cb(this[i],i,this)){
            temp.push(this[i]);   
        }
    }
    return temp;
}

const nums = [1,2,3,4];
// using the new map prototype implementation
const moreThenTwo = nums.myFilter((num,i,arr)=>{
    return num>2;
})

console.log(moreThenTwo) // [3,4]
```

## Interview Question

```js
// map, filter, reduce Interview questions
//Question 1
// return new array with all names in Caps
let students = [
    {name:"Tanuj", rollNumber:31,marks:30},
    {name:"Vijay", rollNumber:45,marks:90},
    {name:"Kishore", rollNumber:52,marks:90},
    {name:"Krishna", rollNumber:20,marks:100},
]

// Using for loop
const names = []
for(let i = 0; i<students.length;i++){
    names.push(students[i].name.toUpperCase());
}
console.log(names) // [ 'TANUJ', 'VIJAY', 'KISHORE', 'KRISHNA' ]

// Using map
const namesMap = students.map((s)=>s.name.toUpperCase());
console.log(namesMap) // [ 'TANUJ', 'VIJAY', 'KISHORE', 'KRISHNA' ]

//Question 2
// return only those values with more than 60 marks
const marksMoreThanSixty = students.filter((s)=>s.marks >60);
console.log(marksMoreThanSixty)
/*
[
  { name: 'Vijay', rollNumber: 45, marks: 90 },
  { name: 'Kishore', rollNumber: 52, marks: 90 },
  { name: 'Krishna', rollNumber: 20, marks: 100 }
]
*/

//Question 3
// return only those values with more than 60 marks & rollnumber >50
const sixtyAndRollNumber = students.filter((s)=> s.marks>60 && s.rollNumber >50);
console.log(sixtyAndRollNumber) // [ { name: 'Kishore', rollNumber: 52, marks: 90 } ]

const totalMarks = students.reduce((acc,curr)=>{
   return acc+curr.marks
},0)
console.log(totalMarks) //310
```