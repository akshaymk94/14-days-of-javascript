# Javascript Essentials

## What is Hoisting in Javascript?

Hoisting is a behavior in Javascript where the variable and function declarations are moved to the top of their containing scope during compilation phase. This makes them accessible within their scope, even before their declaration in code is reached. 

### Variable Hoisting

When Variables in Javascript are declared as 'var', they are hoisted to the top of their function or global scope. This makes them accessible even before their declaration/initialization logic is reached during execution phase. But they are initialized with a special placeholder value 'undefined' until they are assigned a value. 

```javascript
// Variable hoisting with 'var'
console.log(name); // undefined

var name = 'John Smith';
```

### Function Hoisting

Functions in javascript are hoisted entirely, at the top of their scope, making them accessible to call even before the declaration in code. This is made possible by Javascript as it stores the entire definition of the function in memory.

```javascript
// Function hoisting
printName(); // "John Smith"

function printName() {
    console.log("John Smith");
}
```

### Block Scope

In Javascript, Variables can be declared as 'const' or 'let'(introduced in ES6). When declared this way, they are still hoisted at the top of their containing block but remain in a Temporal Dead Zone until their declaration in code is reached. This ensures better scoping behaviour.

```javascript
// 'const' variable in a block scope
console.log(name); // throws ReferenceError: Cannot access 'name' before initialization

const name = "John Smith";
```

```javascript
// 'const' variable in a block scope
const name = "John Smith";

console.log(name); // "John Smith"
```

