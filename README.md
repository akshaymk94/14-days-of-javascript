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

# Javascript - Day 4

## Closures in JavaScript

A Closure in JavaScript is a powerful concept that combines a function with a reference to its surrounding state, known as the lexical environment. The lexical environment includes variables and functions available to the function when it was created.

Reference: MDN Web Docs


https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

Example:
```javascript
function getFullName() {
    var firstName = 'John';
    var lastName = 'Smith';
    function printFullName() {
        console.log(firstName, lastName);
    }
    printFullName();
}

getFullName(); // Output: John Smith
```

In the example above, the printFullName function forms a closure by capturing its surrounding environment. This means it can access variables firstName and lastName declared in its parent environment.

What's crucial to understand is that the closure created by the printFullName function persists even after the getFullName function has finished executing, and its execution context is destroyed. This enables us to do something like:
```javascript
// Utilizing Closure
function getFullName() {
    var firstName = 'John';
    var lastName = 'Smith';
    return function printFullName() {
        console.log(firstName, lastName);
    }
}

const printMyName = getFullName();
printMyName(); // Output: John Smith
```
This magic of closures allows us to create multiple instances, each with its own enclosed state, as shown here:
```javascript

// Example
function getFullName(firstName) {
    return function printFullName(lastName) {
        console.log(firstName, lastName);
    }
}

const printJohnsName = getFullName('John');
printJohnsName('Smith'); // Output: John Smith

const printJillsName = getFullName('Jill');
printJillsName('Doe'); // Output: Jill Doe
```
Practical Example

Closures find extensive use in JavaScript. A common use case is creating closures as callback functions for event handlers, especially when handling user interactions on a webpage. For instance, consider a scenario where a user clicks on buttons to change the color of a paragraph:
```html
// HTML elements
<p id='paragraph'>
Hey, I'm a color-changing paragraph! Choose your color here!
</p>

<button id="btnGreen">
Green
</button>

<button id="btnBlue">
Blue
</button>

<button id="btnRed">
Red
</button>
```
With closures, you can create reusable logic for this:
```javascript

const colorChanger = (color) => {
  return function() {
    document.getElementById('paragraph').style.color = color;
  };
}

const colorGreen = colorChanger('green');
const colorBlue = colorChanger('blue');
const colorRed = colorChanger('red');

document.getElementById('btnGreen').onclick = colorGreen;
document.getElementById('btnBlue').onclick = colorBlue;
document.getElementById('btnRed').onclick = colorRed;
```
In this example, colorChanger creates closures that remember and modify the color of the paragraph even after the initial function call, demonstrating the power and utility of closures in JavaScript.


# Javascript - Day 5

## Emulating private methods with Closure

The concept of closure in Javascript provides us a way to achieve data hiding and encapsulation.
This means we can use a closure to hide/protect sensitive data against unintended access or modification.

Let's understand this concept with an example.

Let's say that I want to build a way to create multiple employees, view their respective details and update their respective details. This can be achieved as shown below:

```javascript
// create a blueprint of the Employee function
const Employee = function (name, yearsOfExperience, title) {
    let details = {
        "name": name,
        "yearsOfExperience": yearsOfExperience,
        "title": title
    }

    function updateDetails(record) {
        details = record;
    }

    return {
        getRecord() {
            return details;
        },
        viewRecord() {
            console.log(details);
        },
        updateRecord(recordReceived) {
            updateDetails(recordReceived);
        }
    }
}

// create an Employee record, name it employeeOne
let employeeOne = Employee("Mary Jane", 6, "SSE");

// verify details of employeeOne
employeeOne.viewRecord(); // {name: 'Mary Jane', yearsOfExperience: 6, title: 'SSE'}

// fetch details of employeeOne
const employeeRecord = employeeOne.getRecord()

// modify the yearsOfExperience and title for employeeOne
const updatedRecord = { ...employeeRecord, yearsOfExperience: 7, title: "Lead" }

// update employeeOne's record
employeeOne.updateRecord(updatedRecord);

// verify details of employeeOne
employeeOne.viewRecord(); // {name: 'Mary Jane', yearsOfExperience: 7, title: 'Lead'}

// Create another Employee Record, name it employeeTwo
let employeeTwo = Employee("John Smith", 2, "SE1");

//view details of employeeTwo
employeeTwo.viewRecord(); // {name: 'John Smith', yearsOfExperience: 2, title: 'SE1'}

// cross verify the details of employeeOne again to ensure data encapsulation
employeeOne.viewRecord(); // {name: 'Mary Jane', yearsOfExperience: 7, title: 'Lead'}

// when you try to access the variable 'details' and function 'updateDetails' of employeeOne, it shows up as undefined.
console.log(employeeOne.details); // undefined
console.log(employeeOne.updateDetails); // undefined
```
