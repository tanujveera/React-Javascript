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
