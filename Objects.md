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

