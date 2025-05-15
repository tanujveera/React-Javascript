# Objects in JS
The `Object` type represents one of JavaScript's data types. It is used to store various keyed collections and more complex entities.

An `Object` initializer is a comma-delimited list of zero or more pairs of property names and associated values of an object, enclosed in curly braces `{}`. Objects can also be initialized using `Object.create()` or by invoking a constructor function with the new operator.

Create object using `{}`
```js
const object1 = { a: "foo", b: 42, c: {} };

console.log(object1.a);
// Expected output: "foo"

const a = "foo";
const b = 42;
const c = {};
const object2 = { a: a, b: b, c: c };

console.log(object2.b);
// Expected output: 42

const object3 = { a, b, c };

console.log(object3.a);
// Expected output: "foo"
```

Create object using `Object()`

```js
const o = new Object();
o.foo = 42;

console.log(o);
// { foo: 42 }
```
---
Accessing Object
- For any strings to access we can use `obj[key]`
```js
// Accessing an element
const object1 = { a: "foo", b: 42, c: {}, "hello world":78 };

console.log(object1.a) // foo
console.log(object1["hello world"]) // 78
```

While creating an object if we use variables as key value pairs
- always use `[]` brackets to the property key to ensure we are using the literal value of that variable as key.

```js
const propertyName = "FirstName"
const name = "Tanuj Veera"

const user = {
  propertyName: name,
}

console.log(user) // { propertyName: 'Tanuj Veera' }

const user = {
  [propertyName]: name,
}
console.log(user) // { FirstName: 'Tanuj Veera' }
```

---
Delete Object
```js
// Accessing an element
const object1 = { a: "foo", b: 42, c: {} };
console.log(object1.a) // foo
console.log(object1["a"]) // foo

// Delete an element
delete object1.b;
console.log(object1) // { a: 'foo', c: {} }
```
- In below code, the `func` will not delete `a` as it is a local variable and not object.
```js
const func = (function (a){
    delete a;
    return a;
})(5);

console.log(func);
```

Accessing keys in Objects

- When we iterate over an object, we get `keys as each item` from for loop.

```js
const user = {
  name:"Tanuj Veera",
  age:24,
  isFlag: true
}

for(key in user){
  console.log(key) // name age isFlag
}

for(key in user){
  console.log(user[key]) // Tanuj Veera 24 true
}
```

# Interview Questions

Q1 What's the output?
- When we duplicate keys then the 2nd value is considered
```js
const obj = {
  a: "one",
  b: "two",
  a: "three"
};

console.log(obj) // { a: 'three', b: 'two' }
```
---
Q2 Create a function multiplyByTwo(obj) that multiplies all numeric property values of nums by 2

```js
const obj = {
  a: 100,
  b:200,
  title:"My nums"
};
multiplyByTwo(obj);

function multiplyByTwo(params) {
  for(key in  params){
    if(typeof params[key] === "number"){
      params[key] = params[key] *2;
    }
  }
  console.log(obj) // { a: 200, b: 400, title: 'My nums' }
}
```
---
Q3 What's the output of this following code?
- When a object itself is assigned as a key it stores as `[objectObject]`. So then we then assign another object as key, its the same key so it overrides it.
```js
const a = {}
const b = {  key: "b"}
const c = {  key: "c"}

a[b] = 123; // a['[object Object]'] = 123
a[c] = 456; // a['[object Object]'] = 456

console.log(a) // { '[object Object]': 456 }
console.log(a[b]) // 456
```
---
Q4 What is **JSON.stringify() & JSON.parse()** ?
- Use case: We can store a JSON in string format in `local storage` in browser since we can't store JSON directly.
```js
const user = {
  name: "Piyush",
  age: 24,
};
const strObj = JSON.stringify(user) // Converts the JSON to string
console.log(user) // { name: 'Piyush', age: 24 }
console.log(strObj)// '{"name":"Piyush","age":24}'
console.log(JSON.parse(strObj)) // { name: 'Piyush', age: 24 }
```
Local Storage
- In browser Inspect tab > Application > Local Storage, you will see a `test` key and value as `{"name":"Piyush","age":24}`
```js
const user = {
  name: "Piyush",
  age: 24,
};
const strObj = JSON.stringify(user)
localStorage.setItem("test",strObj)
```
---
Q5 What is output for this? 

Spread operator
```js
console.log([..."Tanuj"]) // [ 'T', 'a', 'n', 'u', 'j' ]
```
Q6 Whats the output? Spread opearator
```js
const user = {
  name: "Piyush",
  age: 24,
};
const admin = {
  admin: true,
  ...user
}
console.log(admin) // { admin: true, name: 'Piyush', age: 24 }
```

Q7 Only consider `name age` to stringify

```js
const user = {
  name: "Piyush",
  age: 24,
  username:"piyush"
};
const data = JSON.stringify(user,["name","age"])
console.log(data) // {"name":"Piyush","age":24}
```

Q8 What is the output? Function scope / this scope
- Here functions have local scope of the shape object but the arrow functions `this` consider the global scope.
```js
const shape = {
  radius:20,
  diameter() {
    return this.radius *2;
  },
  perimeter: ()=> 2* Math.PI * this.radius,
}
console.log(shape.diameter()); // 40
console.log(shape.perimeter()); // NaN
```

Q9 What is destructuring in objects?

```js
let user = {
  name:"Tanuj",
  age:24
};

const {name: myName} = user;

console.log(myName) // Tanuj
```
Destructuring a nested object
```js
let user = {
  name:"Tanuj Veera",
  age:24,
  fullName: {
    first: "Tanuj",
    last:"Veera",
  }
};

const {name, fullName :{first}} = user;

console.log(name) // Tanuj Veera
console.log(first) // Tanuj
```
---
Q10 Rest Operator Output
- `Rest` operator should be always used at the last.
- `Spread` operator can be used anywhere. 
```js
function getItems(fruitList, favoriteFruit,...args){
  return [...fruitList,...args,favoriteFruit]
}
console.log(getItems(["banana","apple"],"pear","orange"))
// SyntaxError: Rest parameter must be last formal parameter
```
By correcting the code, adding the rest operator at the last

```js
function getItems(fruitList, favoriteFruit,...args){
  return [...fruitList,...args,favoriteFruit]
}
console.log(getItems(["banana","apple"],"pear","orange"))
// [ 'banana', 'apple', 'orange', 'pear' ]
```
---
Q11 What is the output? Call by Value

- If you see, primitive types like `number`, `string`, `boolean`, `undefined`, `null`, `symbol`, `bigint` have their values copied. when we check those 2 variables will hold different values, when values are changed.
- For objects, they are referenced to the object in memory. so both of those variables will point out to the same object in memory. If value is changed through one of the object,since other object points to the same, it will have the same value changed.
```js
let c = { greeting: "Hey!" }
let d;

d=c;
c.greeting = "hello";
console.log(d.greeting,c.greeting) //  hello hello

let a = 10
let b;

b=a;
a=9
console.log(a,b) // 9 10

function change(obj) {
  obj = { msg: "New" };
}

let test = { msg: "Old" };
change(test);
console.log(test.msg); // Old
```

**Call by Value**
- JavaScript always uses Call by Value — but the meaning of "value" differs between primitives and objects.

Primitives
```js
let a = 10;
let b = a;
a = 20;

console.log(a); // 20
console.log(b); // 10
```
- `a` and `b` are independent.

- `b = a` copies the value of a (10) into `b`.

- **Call by value**: you pass the actual value, and changes don't affect the original.

**Objects/Arrays/Functions → Still Call by Value (of reference)**

```js
let obj1 = { msg: "Hello" };
let obj2 = obj1;

obj1.msg = "Hi";

console.log(obj2.msg); // Hi
```
- `obj1` holds a reference (address) to the object.

- When you do `obj2 = obj1`, you're copying the reference, not the object itself.

- So `obj1` and `obj2` both point to the same object in memory.

- Still `call by value` — but the value being passed or assigned is a reference to the object.

**JS doesn't have Call by Reference**

True Call by Reference means that if you pass a variable to a function, and the function reassigns it, the original variable changes.

```js
// Objects
function change(obj) {
  obj = { msg: "New" };
}

let test = { msg: "Old" };
change(test);
console.log(test.msg); // Still "Old"

// Primitive types
function change(obj) {
  obj = 9;
}

let test = 8;
change(test);
console.log(test); // 8
```

- Inside the function, obj was reassigned to a new object.

- The outer test still points to the original object — unchanged.

- Proof that JS is not truly `Call by Reference`
---