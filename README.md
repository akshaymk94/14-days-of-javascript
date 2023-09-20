# Javascript - Day 1

## Hoisting in Javascript

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

# Javascript - Day 2

## Lexical Environment and Scope Chain in JavaScript

Lexical Environment is a fundamental concept in JavaScript that organizes variables and functions along with their associated values within specific scopes.

A Lexical Environment consists of two key elements:

- Environment Record: Within a Lexical Environment, the Environment Record acts as a repository where variables and functions are mapped to their respective values within a defined scope.

- Reference to Outer Environment: Alongside the Environment Record, each Lexical Environment also maintains a reference to its outer environment. This linkage creates a hierarchy of Lexical Environments, crucial for comprehending variable scope and closures in JavaScript. When a variable's value is not found in the current environment, the JavaScript Engine explores the outer environments until it locates the variable or reaches the global scope.

In JavaScript, the execution of code occurs within specific contexts. When a program runs, a global environment(a.k.a Global Execution Context) is established. As functions are invoked during program execution, new environments are created to handle the respective functions. This leads to formation of a hierarchy of environments.

Imagine you're attempting to access a variable deep within a nested function. The JavaScript engine first searches for this variable in the local memory (Environment Record) of the current environment. If it finds the variable, it uses it for subsequent operations. However, if the variable isn't found, the JavaScript engine examines the memory of its enclosing environment (utilizing the Reference to the Outer Environment). This search continues until the variable is found or until the JavaScript engine reaches the global environment.

Scope Chain refers to the sequence of lexical environments that the JavaScript Engine traverses when resolving the value of a variable. It initiates from the current local scope and extends outward to encompass the containing scopes, following the lexical hierarchy until it discovers the variable's value or reaches the global scope.

Understanding Lexical Environments and the Scope Chain is fundamental for writing JavaScript code with precise variable scoping and for avoiding unintended variable conflicts.

# Javascript - Day 3

## Block Scope and Shadowing in Javascript

A Block in Javascript is a compound statement, which means two or more Javascript statements are grouped together using {curly braces} to form a block.

```javascript
{
    let myVar = 10;
    console.log("myVar contains value : ", myVar);
}
```

Block is generally used in use cases where we need to execute multiple JS statements but Javascript Syntax allows only one JS statement.

```javascript
// Normally, an if condition accepts only one statement when a condition is true.
if (true) console.log("condition is true");

// To execute multiple statements when the condition is true, you can use a block like this:
if (true) {
    let myVar = 10;
    console.log("myVar contains value:", myVar);
}
```

A Scope in Javascript refers to the boundary in memory within which Javascript variables and functions are accessible.

```javascript
var myVar = 10;

{
    // Variables declared with 'let' have block scope, making them available only within this block.
    let myLet = 20;

    console.log(myLet); // Outputs 20; works fine
}

console.log(myLet); // Throws ReferenceError: myLet is not defined
```

Block Scope refers to a specialized memory allocation for 'let' and 'const' variables declared within a block. It acts as a boundary within which these variables are accessible. Block scope is created when the JavaScript engine begins executing a block and is discarded once the block's execution concludes.

```javascript
{
    let blockVar = 10; // blockVar is only accessible within this block
    console.log("blockVar:", blockVar); // Outputs 10
}

console.log(blockVar); // Throws ReferenceError: blockVar is not defined
```


Shadowing occurs when a variable defined within a specific block has the same name as a variable defined in the containing/global scope. In such cases, the variable in the containing/global scope is shadowed by the variable declared within the block.

```javascript
let outerVar = 20;

{
    let outerVar = 10; // This 'outerVar' shadows the outer one within this block
    console.log("Inner block outerVar:", outerVar); // Outputs 10
}

console.log("Outer scope outerVar:", outerVar); // Outputs 20, the outer variable is unaffected
```

Illegal shadowing is when a 'let' variable is declared in the global scope and a 'var' variable with the same name is declared within a block. This logic throws error as the 'var' variable, even though declared inside a block, has global scope, thereby conflicting with the 'let' variable which is already declared in global scope.

```javascript
let globalLet = 30;

{
    var globalLet = 40; // Uncaught SyntaxError: Identifier 'globalLet' has already been declared
    console.log("Inside block globalLet:", globalLet); 
}

console.log("Outside block globalLet:", globalLet);
```