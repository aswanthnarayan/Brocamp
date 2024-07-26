# JAVASCRIPT
JavaScript is a Dynamic,Single-threaded ,Interpreted (code executed line by line by an interpreters) Programming language with First class Functions ,It is most well known scripting language for Web pages ,also many non-browser environment also use it like Node.js .

**First class Functions :** Functions that are treated like any other variables in a programming language called first class functions.

## JavaScript Engines
JavaScript engines are the programs or interpreters that execute JavaScript code.
examples:

1. **V8** (Chrome,Node.js,Opera)
2. **SpiderMonkey** (Firefox)
3. **JavaScriptCore** (Safari,Webkit)
4. **Chakra** (Microsoft Edge)



# FOUNDATIONS

## VARIABLES
Variables are containers for storing data values.You can declare a variable with **var**, **let** or **const** keywords.
* Variable names include letters,digits, underscores, and dollar signs
* Variable names can't start with letters.
* JS is case-sensitive language , so `myVar` and `myvar` are different variable.
* Always prefer `let`,and `const` over `var` for avoiding scoping and hoisting issues

```js

let myVariable;

```
## SCOPE
In JavaScript Scope refers to the context in which variables and functions are accessible 
JS has two main type of scope , `Global Scope` and `Local Scope`

### Global Scope
Variable declared outside any function or block are in the global scope.They can be accessed from anywhere in the code.
```js
var globalVariable = "Iam Global Variable"

function accessGlobal(){
console.log(globalVariable) // Accessible
}
accessGlobal();
console.log(globalVariable); // Accessible
```

### Local Scope
Local scope refers to variables that are accessible only with in specific part of the code , such as function or block;

**Function Scope**
Variables declared inside a function using `var`, `let`, or `const` are in function scope. They are not accessible outside the function.

```js
function myFunction() {
  var functionScoped = "I'm inside a function";
  console.log(functionScoped); // Accessible
}

myFunction();
console.log(functionScoped); // Not accessible, ReferenceError
```

**Block Scope**
Variables declared inside a block (e.g., inside `{}`) using `let` or `const` are block-scoped. They are accessible only within that block.
```js
if (true) {
  let blockScoped = "I'm inside a block";
  console.log(blockScoped); // Accessible
}

console.log(blockScoped); // Not accessible, ReferenceError
```
### Lexical Scope
JavaScript uses lexical scoping, meaning the scope of a variable is determined by its position within the source code. Nested functions have access to variables declared in their outer scope.

```js
function outerFunction() {
  var outerVariable = "I'm outside!";
  
  function innerFunction() {
    console.log(outerVariable); // Accessible due to lexical scope
  }
  
  innerFunction();
}

outerFunction();
```
**LEXICAL ENVIRONMENT**
A lexical environment in JavaScript refers to the structure that holds variables, function declarations, and the scope in which they exist.

### Scope Chain
When a variable is referenced, JavaScript looks up the variable name in the current scope. If it's not found, it moves to the outer scope, and so on, until it reaches the global scope. This chain of scopes is called the scope chain.

### Shadowing
When a variable in a local scope has the same name as a variable in an outer scope, the local variable "shadows" the outer one. The inner variable takes precedence within its scope.

```js
var shadowVar = "Global";

function shadowTest() {
  var shadowVar = "Local";
  console.log(shadowVar); // Logs "Local"
}

shadowTest();
console.log(shadowVar); // Logs "Global"
```

## HOISTING IN JS
In JavaScript, hoisting is a behavior in which variable and function declarations are moved, or "hoisted," to the top of their containing scope (either global or function scope) during the compile phase before the code is executed. This means that you can use functions and variables before you declare them in the code

### VARIABLE HOISTING
When variables are hoisted, only the declarations are hoisted to the top, not the initializations. If you try to access a variable before it is initialized, it will be `undefined`.
**Example with var:**
```js
console.log(hoistedVar); // Output: undefined
var hoistedVar = "I am hoisted";
console.log(hoistedVar); // Output: "I am hoisted"
```
Internally, this code is interpreted as:
```js
var hoistedVar;
console.log(hoistedVar); // Output: undefined
hoistedVar = "I am hoisted";
console.log(hoistedVar); // Output: "I am hoisted"
```
**Example with let and const:**
Variables declared with `let` and `const` are also hoisted, but they are not initialized. Accessing them before the declaration results in a `ReferenceError`.

```js
console.log(hoistedLet); // ReferenceError: Cannot access 'hoistedLet' before initialization
let hoistedLet = "I am hoisted";

console.log(hoistedConst); // ReferenceError: Cannot access 'hoistedConst' before initialization
const hoistedConst = "I am hoisted";
```

**Check functions section before for better understanding**

## FUNCTION HOISTING
Function declaration are fully hoisted meaning name and it's body are moved to the top of their containing scope. This allow you to call function before its declaration 

**Example with Function Declarations:**
```js
hoistedFunction(); // Output: "I am hoisted"

function hoistedFunction() {
  console.log("I am hoisted");
}
```
Internally, this code is interpreted as:
```js
function hoistedFunction() {
  console.log("I am hoisted");
}

hoistedFunction(); // Output: "I am hoisted"
```

**Example with Function Expressions:**
Function expressions, including those created with `var`, `let`, or `const`, are not hoisted in the same way. Only the variable declaration is hoisted, not the function assignment.
```js
hoistedExpression(); // TypeError: hoistedExpression is not a function
var hoistedExpression = function() {
  console.log("I am not hoisted");
};
```
Internally, this code is interpreted as:
```js
var hoistedExpression;
hoistedExpression(); // TypeError: hoistedExpression is not a function
hoistedExpression = function() {
  console.log("I am not hoisted");
};

```

## DATA TYPES IN JS

### PRIMITIVE DATA TYPES
Primitive data types are most basic data types, They are immutable meaning cant change values after they are created

1. **Number:** Represent integer and float numbers
2. **Strceing:** Represent sequence of characters
3. **Boolean:** Represent logical values ie `true` or `false`
4. **Null:** Represent intentional absence 
5. **Undefined:** Represent a variable that has been declared but not assigned a value;
6. **Symbol:** Represents a unique and immutable identifier.

### NON-PRIMITIVE DATA TYPES
Non primitive data types are objects. Unlike primitive data types , they can hold collections of values and more complex entities

1. **Object**
Represent a collection of `key-value` pair 
```js
let person = {
  name: 'John',
  age: 30
};
```
2. **Array**
Represents an ordered collection of values (indexed).
Example: `let numbers = [1, 2, 3, 4, 5];`

3. **Function**
Represents a block of code designed to perform a particular task.
```js
function greet() {
  return 'Hello';
}
```
4. **Date**
Represents a single moment in time.
Example: `let now = new Date();`

5. **RegExp**
Represents regular expressions, used for pattern matching within strings
Example: `let pattern = /ab+c/;`

6. **Error**
Represents runtime errors.
```js
try {
  throw new Error('Something went wrong');
} catch (e) {
  console.log(e.message); // 'Something went wrong'
}
```

## OPERATORS

### ARITHMETIC OPERATORS
Arithmetic operators are used to perform mathematical operations.

* **Addition (+):** Adds two numbers.
* **Subtraction (-):** Subtracts one number from another.
* **Multiplication (*):** Multiplies two numbers.
* **Division (/):** Divides one number by another.
* **Modulus (%):** Returns the remainder of a division.
* **Exponentiation ()****: Raises the first operand to the power of the second operand.
* **Increment (++):** Increases a variable's value by one.
* **Decrement (--):** Decreases a variable's value by one.

### ASSIGNMENT OPERATORS
Assignment operators are used to assign values to variables.

* **Assignment (=):** Assigns a value to a variable.
* **Addition Assignment (+=):** Adds and assigns a value.
example:
```js
let x = 5;
x += 3; // 8
```
* **Subtraction Assignment (-=):** Subtracts and assigns a value.
* **Multiplication Assignment (*=):** Multiplies and assigns a value.
* **Division Assignment (/=):** Divides and assigns a value
* **Modulus Assignment (%=):** Takes modulus and assigns a value.
* **Exponentiation Assignment (=)****: Raises to a power and assigns a value.

### COMPARISON OPERATORS
Comparison operators are used to compare two values and return a boolean result.

* **Equal (==):** Compares values for equality (type conversion allowed).
* **Strict Equal (===):** Compares values and types for equality.
* **Not Equal (!=):** Compares values for inequality (type conversion allowed).
* **Strict Not Equal (!==):** Compares values and types for inequality.
* **Greater Than (>):** Checks if the left operand is greater than the right operand.
* **Less Than (<):** Checks if the left operand is less than the right operand.
* **Less Than or Equal (<=):** Checks if the left operand is less than or equal to the right operand.

### LOGICAL OPERATORS
Logical operators are used to combine or invert boolean values

* **Logical AND (&&):** Returns true if both operands are true.
* **Logical OR (||):** Returns true if at least one operand is true.
* **Logical NOT (!):** Inverts the boolean value.

### BITWISE OPERATORS
Bitwise operators perform operations on the binary representation of numbers

* **AND (&):** Performs a bitwise AND.
```js
5 & 1; // 1 (0101 & 0001)
```
* **OR (|):** Performs a bitwise OR.
```js
5 | 1; // 5 (0101 | 0001)
```
* **XOR (^):** Performs a bitwise XOR.
```js
5 ^ 1; // 4 (0101 ^ 0001)
```
* **NOT (~):** Performs a bitwise NOT.
```js
~5; // -6 (~0101 = 1010)
```
* **Left Shift (<<):** Shifts bits to the left.
```js
5 << 1; // 10 (0101 << 1 = 1010)
```
* **Right Shift (>>):** Shifts bits to the right.
```js
5 >> 1; // 2 (0101 >> 1 = 0010)
```
* **Unsigned Right Shift (>>>):** Shifts bits to the right, filling with zeros.
```js
5 >>> 1; // 2 (0101 >>> 1 = 0010)
```

### STRING OPERATORS
In addition to the arithmetic addition operator, +, which concatenates strings, JavaScript also provides other string-related operations.

```js
let greeting = 'Hello' + ' ' + 'World!'; // "Hello World!"
```

### CONDITIONAL (TERNARY) OPERATOR
The conditional operator assigns a value based on a condition.
```js
let age = 18;
let canVote = (age >= 18) ? 'Yes' : 'No'; // "Yes"
```
### TYPE OPERATORS
**typeof:** Returns the type of a variable.
```js
typeof 5; // "number"
```

## CONDITIONAL STATEMENTS
Conditional statements allow you to perform different actions based on different conditions.

1. **IF ELSE Statements**
The `if...else` statement allows you to execute code based on a condition. You can chain multiple conditions using `else if`
```js
let num = 5;

if (num > 10) {
  console.log("Number is greater than 10");
} else if (num > 5) {
  console.log("Number is greater than 5 but less than or equal to 10");
} else if (num > 0) {
  console.log("Number is greater than 0 but less than or equal to 5");
} else {
  console.log("Number is 0 or less");
}
```

2. **SWITCH Statements**
The `switch` statement is another way to handle multiple conditions. It's useful when you have many conditions based on the same variable.

```js
let fruit = "apple";

switch (fruit) {
  case "banana":
    console.log("Banana is yellow");
    break;
  case "apple":
    console.log("Apple is red or green");
    break;
  case "orange":
    console.log("Orange is orange");
    break;
  default:
    console.log("Unknown fruit");
}
```

## LOOPS
Loops allow you to repeat a block of code multiple times.

1. **FOR Loop**
The `for` loop is the most common type of loop in JavaScript
```js
for (let i = 0; i < 5; i++) {
  console.log("Iteration number " + i);
}
```
2. **WHILE LOOP**
The `while` loop executes a block of code as long as a specified condition is true.
```js
let i = 0;
while (i < 5) {
  console.log("Iteration number " + i);
  i++;
}
```
3. **DO WHILE LOOP**
The `do...while` loop is similar to the while loop, but it guarantees that the block of code is executed at least once, even if the condition is false.
```js

let j = 0;
do {
  console.log("Iteration number " + j);
  j++;
} while (j < 5);

```
4. **FOR IN LOOP**
The `for...in` loop is used to iterate over the properties of an object.

```js
let person = {name: "John", age: 30, city: "New York"};

for (let key in person) {
  console.log(key + ": " + person[key]);
}

```
5. **FOR OF LOOP**
The `for...of` loop is used to iterate over iterable objects like arrays, strings, and NodeLists.

```js
let numbers = [1, 2, 3, 4, 5];

for (let num of numbers) {
  console.log(num);
}
```

## FUNCTIONS IN JS
A JavaScript function is a block of code designed to perform a particular task.It encapsulates a set of instructions that can be reused throughout a program. Functions can take parameters, execute statements, and return values, enabling code organization, modularity, and reusability in JavaScript programming.

### Function Declaration
A function declaration defines a named function in JavaScript 

```js
function greet() {
    console.log("Hello!");
}
greet(); // "Hello!"

```

### Function Expressions
A function expression is when a function is assigned to a variable. This contrasts with function declarations, which are standalone functions

```js
const greet = function() {
    console.log("Hello!");
};
greet(); // "Hello!"
```

**Key Differences:**
* **Hoisting**: Function declarations are hoisted, meaning they can be called before they are defined in the code. Function expressions are not hoisted.
* **Naming:** Function declarations must have a name, whereas function expressions can be anonymous.

## ARROW FUNCTION
An arrow function expression is a compact alternative to a traditional function expression, with some semantic differences and deliberate limitations in usage

```js
const functionName = (parameter1, parameter2, ...) => {
    // function body
};
```
One of the most significant features of arrow functions is that they do not have their own this context. Instead, they inherit this from the parent scope at the time they are defined. This behaviour is known as lexical scoping.

```js
function Person() {
    this.age = 0;

    setInterval(() => {
        this.age++;
        console.log(this.age);
    }, 1000);
}

const person = new Person();
```




































