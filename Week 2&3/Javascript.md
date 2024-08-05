# JAVASCRIPT
JavaScript is a Dynamic,Single-threaded ,Interpreted (code executed line by line by an interpreters) Programming language with First class Functions ,It is most well known scripting language for Web pages ,also many non-browser environment also use it like Node.js .

**JavaScript is a dynamically typed Language** ie, variable types are determined at runtime so we dont want to specify data type 

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
## LET VAR CONST
In JS `let` , `var` , `const` used to declare variables. but they have diffrent behaviors and scopes 

### VAR
* var is is function-scoped, meaning it is accessible within the function it is declared in. If declared outside any function, it is globally scoped 
* Variables declared with var are hoisted to the top of their scope and initialized with undefined
*Variables declared with var can be re-declared within the same scope.

### LET
* let is block-scoped, meaning it is only accessible within the block it is declared in (e.g., within {}).
* Variables declared with let are hoisted but not initialized, leading to a Temporal Dead Zone (TDZ) until the declaration is encountered
*  Variables declared with let cannot be re-declared within the same scope.

### CONST
* const is block-scoped, similar to let.
* Variables declared with const are hoisted but not initialized, leading to a Temporal Dead Zone (TDZ) until the declaration is encountered.
* Variables declared with const cannot be re-declared within the same scope.
* const requires an initial value at the time of declaration, and the variable reference cannot be reassigned. However, the contents of objects and arrays declared with const can be modified.


## Temporal Dead Zone (TDZ)
The Temporal Dead Zone (TDZ) in JavaScript is a behavior that occurs with variables declared using let and const before they are initialized. The TDZ refers to the period of time from entering a scope to the point where the variable is declared and initialized. During this period, accessing the variable results in a ReferenceError.


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
### UNDEFINED VS NOT DEFINED
A variable is said to be `undefined` if it has been declared but not yet assigned a value. This means the variable exists in the current scope but does not have an initialized values

`notdefined` means The variable has not been declared in the current scoped
this leads into reference errors

## Nan
In JS Nan stands "for Not a Number" , and means by computational results cant be expressed as a meaningful Number

**isNan function** -- this function convert the value to a number and checks if it is a `Nan` 

```js
console.log(isNaN(NaN)); // Output: true
console.log(isNaN("hello")); // Output: true (because Number("hello") is NaN)
console.log(isNaN(123)); // Output: false
```

## Strict mode
Strict mode is introduced in es5 in JS it is a way to opt into a restricted varient of JS , which help you to catch common coding mistakes and "unsafe" actions such as defining a global variable unintentionally.

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
The `for...of` loop is used to iterate over iterable objects like arrays, strings, and Node Lists.

```js
let numbers = [1, 2, 3, 4, 5];

for (let num of numbers) {
  console.log(num);
}
```

## ERRORS in JS
In  errors are categorized into different types , each represent specific kind of problems occuring during the execution of the code
The main Types are
**Syntax Errors** - Occur when code does not conform to the syntax of the language
**Reference Errors** - Occur when try to reference a variable that does not exist or initialized in the current scope
**Type Error** -Occur when the value is not a expecd type 
**Range Error** Occur when a value is not with in  the set or range of allowed values , ex: passing an invalid length to array like -1

## CLOSURE in JS
Closure is the combination of function bundled together with references to its lexical environment . in other words a closure gives you access to an outer function scope from inner function

ex:
```js
function x(){
var a =7
function y(){
console.log(a)
return y;
}
var z = x();
/////
after severl lines we can still access a inside x by calling z funxtion

console.log(z) ; //output will be 7
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

### HIGHER ORDER FUNCTIONS
Higher order functions are functions that operate on other functions by taking them as arguments , returning them or both
* A Higher order function can take one or more function as arguments
* A Higher order function can also return a function as a result
* Higher-order functions are commonly used for callbacks in asynchronous programming

examples    : `map` , `fiter` , `reduce`


### Eval Functions
The Eval function in JS is able to read strings which must be an expression or formula . It basically reads and evaluate if tng might be a calculation
In strict mode, declaring a variable named eval or re-assigning eval is a SyntaxError
example:

```js
console.log(eval('2 + 2'));
// Expected output: 4
```

### Pure Functions
Its is a function that does not have side effects and a function that return exact same thing every time that function invoked with same input

```js
const array = [1,2,3];

function addElementToArray(a,element){
return [...a,element]
}
```

### IIFE (Immediately Infoked Function Expressions)
IIFE is a JS function that run as soon as the function declared

```js
(function(){
var a =3;
console.log(3)
})()
```

### Generator Functions 
Regular function only return only once ,Generator function can return multiple values , one after another on demand
To create generator function use special syntax `function*`

```js
function* generaterFunction(){
    yeild 10;
    yeild 20;
    yeild 30;
}
//Calling generate Function

let gen = generaterFunction();

console.log(gen.next().value);
console.log(gen.next().value);
console.log(gen.next().value);

```
* The `yield` operator is used to pause and resume a generator function.
* `next()` is the main method for generate function , the result of next() is always object with 2 properties
1. `value` : the yeilded (returned) value
2. `done` : if the function is completly completed it will be `true` else `false`


### Factory Function
Its just a function create an object and return that object , They offer a flexible way to create objects without using `class` or `new`.  They can encapsulate private variables and functions.

```js
function createPerson(name, age) {
  return {
    name: name,
    age: age,
    greet: function() {
      console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
    }
  };
}

const person1 = createPerson('Alice', 30);
const person2 = createPerson('Bob', 25);

person1.greet(); // Output: Hello, my name is Alice and I am 30 years old.
person2.greet(); // Output: Hello, my name is Bob and I am 25 years old.
```

## OBJECTS
Objects is one of the data types in JS , it used to store values in **key:value** pairs , Objects are mutable in JS 

example:
```js
const person = {
  firstName: "John",
  lastName : "Doe",
  id       : 5566,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
}
```
Objects can also create using `new` keyword.
We can access object properties in tow ways
1. objectName.propertyName
2. objectName["propertyName"]


The `delete` keyword delete property from an object in above case 

` delete person.id ; ` 

### Object Methods
Object methods are actions that can performed on objects

There are several methods to convert an object into array, the main advantage of this , this process make an object iterable(ie we can loop through this)

consider below object as examle

```js
const obj = {
    cat : 'meow',
    dog : 'bow bow',
    cow : 'meh...'
}
```
1. **Object.keys**
Using `Object.keys` and passing object will return a array with that object keys

example:

```js
console.log(Object.keys(obj));
// returns ["cat","dog","cow"]
```

2. **Object.values** 
Using `Object.values` and passing object will return a array with that object values


example :

```js
console.log(Object.values(obj);
//returns ["meow", "bow bow","meh.."] 
)
```
3. **Object.entries**
Using `Object.entries` and passing object will return an 2D Array with key and of that Object

example:

```js
console.log(Object.entries(obj))
//returns [["cat","meow"],
           ["dog","bow bow"],
           ["cow","meh..."]
          ]
```


## This Keyword
In JS `this` keyword is reference to something like object. 
but this keyword work diffrently in diffrent senarios , like check in case of an object with normal function

```js
let user ={
name= 'Aswanth',
age:26,
function getName(){
    console.log(this.name)
}
}
user.getName() //print Aswanth in console
```
lets go into more complex object 

```js
let user ={
name= 'Aswanth',
age:26,
childObj:{
    nickname:"Ash"
    function getName(){
    console.log(this.name ,"and",this.nickname)
}
}
}
user.childObj.getName() //print 'undefined and Ash' in console

```
in this case the `this` in referenced only to childObj , ie incase of normal function `this` only referenced its direct parent

Now Lets check the same with Arrow functions

```js
let user ={
name= 'Aswanth',
age:26,
getName:()=>{
    console.log(this.name)
}  
}
user.getName() //print nothing

```

`this` keyword value in arrow function referenced its par parent function , in above case there is no parent function so it point to the window object 
, to access name in this case need to use nested arrow function

```js
let user = {
  name: "Aswanth",
  age: 24,
   getName(){
        const nestedArrow = () => console.log(this.name); //Aswanth
        nestedArrow();
    }
};
```
## CALL APPLY BIND
These methods are powerful tools in JavaScript for function borrowing and ensuring that functions execute in the desired context.
1. **CALL**
Using `call` method, we can borrow a function from one object and use it for another object.

Consider an object 

```js
let name = {
    firstname: "Aswanth",
    lastname: "Ck",
    printFullname: function() {
        console.log(this.firstname + " " + this.lastname);
    }
}
```
Now, if we need to create a similar object with the same properties (`firstname`, `lastname`) and `printFullname` function, instead of repeatedly writing the function in the new object, we can borrow the `printFullname` function directly from the name object.

the syntax is 
```js
let name2 = {
    firstname: "Yuvraj",
    lastname: "Singh"
}

name.printFullname.call(name2);

```

Usually, we don't store these types of functions inside objects. Instead, we make them global and use them accordingly:

```js
 let printFullname=function(){
    console.log(this.firstname+ " "+ this.lastName)
    } 

printFullname.call(name);

```
You can add more parameters to this function. When doing so, always pass arguments after the object:

```js
 let printFullname=function(hometown){
    console.log(this.firstname+ " "+ this.lastName + "from "+ hometown)
    } 

printFullname.call(name,"Kannur");

```

2. **APPLY** 

The `apply` method works similarly to `call`. The only difference is that instead of passing arguments individually in the `apply` method, you pass them as an array.

```js

let printFullname = function(hometown, state, country) {
    console.log(this.firstname + " " + this.lastname + " from " + hometown + ", " + state + ", " + country);
}

printFullname.apply(name, ["Kannur", "Kerala", "India"]);


```

3. **BIND**
The `bind` method creates a copy of the function and binds it with the object. It doesn't invoke the function directly; instead, it stores the function in a variable so you can call it later.

```js
let printFullname = function(hometown) {
    console.log(this.firstname + " " + this.lastname + " from " + hometown);
}

let printName = printFullname.bind(name, "Kannur");

printName(); // This will invoke the function


```

## CURRYING
Currying is an function that take one argument at a time and return a new function that expecting the next argument






























