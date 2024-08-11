# Node.js

Node.js is an open-source, cross-platform JavaScript runtime environment that allows you to build end-to-end JavaScript applications.

With Node.js, you can run JavaScript code outside the browser. However, in the Node.js environment, you don't have access to some Web APIs like fetch .

Instead, Node.js provides lower-level computing capabilities, such as accessing the file system, through C++ bindings using the V8 engine.which is the same engine that powers Google Chrome. This engine compiles JavaScript directly to native machine code, which is part of why Node.js is so fast.
Node.js consists of C++ files that form the core features, and JavaScript files that expose some common utilities and C++ features for easier use.

![NodeJS Runtime](https://raw.githubusercontent.com/aswanthnarayan/Brocamp/main/Daigrams/NodeJS_Runtime.png)

## JavaScript Engine

A JavaScript engine converts JavaScript code into machine code, enabling the computer to perform specific tasks.

## JavaScript Runtime

A JavaScript runtime is an environment that provides all the necessary components to run a JavaScript program. Every browser has a JavaScript engine, which is a key component of the JavaScript runtime.

## Browser vs. Node.js

In the browser, most of the time, we interact with the DOM or other Web Platform APIs like Cookies. In contrast, Node.js provides APIs through its modules, such as filesystem access, which are not available in the browser.

## MODULES

A module is a encapsulated and reusable chunk of code that has its own context.
In Node Js each file is treated as a separate modules

### TYPES OF MODULES

1. **LOCAL MODULE:** Modules that are created in our application
2. **BUILT-IN-MODULES** Modules that Node.js ships with out of the box.
3. **THIRD PARTY MODULES:** MOdules written by other developers that can use in our application

#### Local Modules

Modules that we create and use in our applications

1.

```js
//index.js

console.log("Hello world"

```

2.

```js
//add.js

const add = (a, b) => {
  return a + b;
};

const sum = add(1, 2);

console.log(sum);
```

Consider these two as a separate module , we can access the `add.js` inside the `index.js` using inbuilt `**require()**` function by Node.js.

```js
//index.js

require("./add.js"); //or require("./add")

console.log("Hello world");
```

the output of executing `index.js` will be

```
//output

3
Hello world

```

This is happen because of **Common JS**

**Common JS:** Common JS is a global standard that how module will be structured and shared

But this way of accessing one module to another prevent reusability of certain function
to overcome this we can use **module.export()** object.

### module.exports

`module.exports` is an in build object that allows to export functions , objects or variable from one module to another
we can export multiple thing as properties of object or a single item directly
other modules can access these export using `require` function

```js
//add.js

const add = (a, b) => {
  return a + b;
};

module.exports = add;
```

```js
//index.js

const add = require(""./add")

console.log("Hello world")

const sum = add (1,2);

console.log(sum)

const sum2 = add (8,2);

console.log(sum2)


```

the output will be

```
Hello world
3
10

```

- when we access a module using `require` from `module.export` we don't need to use same name as export , the name can be any thing while requiring

in this case

```js

const addFunction = require(""./add")

```

this also works fine . this type of export called **Default Exports**

You can Pass Multiple functions also using `module.exports`

```js
// math.js

function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

//1
module.exports = {
  add: add,
  subtract: subtract,
};

//2 or using Object destructuring
module.exports = { add, subtract };
```

Now, in another file, you can import and use these functions:

```js
// app.js

const math = require("./math");

console.log(math.add(5, 3)); // Outputs: 8
console.log(math.subtract(5, 3)); // Outputs: 2
```

### exports

`exports` is a short hand or ails to `module.exports`

```js
// greetings.js

exports.sayHello = function (name) {
  return `Hello, ${name}!`;
};

exports.sayGoodbye = function (name) {
  return `Goodbye, ${name}!`;
};
```

```js
// app.js

const greetings = require("./greetings");
console.log(greetings.sayHello("Alice")); // Outputs: Hello, Alice!
console.log(greetings.sayGoodbye("Bob")); // Outputs: Goodbye, Bob!
```

- `exports` and `module.exports` both point to the same object

- The key deference is Assigning a new value to `exports` break connection to `module.exports`

- Assigning a New Value to exports breaks the link between exports and module.exports, so only the original module.exports object (which is likely empty) gets exported.

```js
// greetings.js

module.exports = function (name) {
  return `Hello, ${name}!`;
};
```

```js
// app.js

const greetings = require("./greetings");
console.log(greetings); // Outputs: {}
```

So for Replacing the Export Always use `module.exports` if you want to replace the entire exported value with something new (like a function, object, or class).

## MODULE SCOPE

Each module in Node.js has its own scope. Before a module code is executed Node.js will wrap it inside a function Wrapper that provide a module scope

This method help to avoid conflicts while running code
Also the proper encapsulation and reusability is guaranteed with it

To create the module scope, Node.js uses an Immediately Invoked Function Expression (IIFE) method internally.

example:

```js
//batman.js

const SuperHero = "Batman";

console.log(SuperHero);
```

```js
//superman.js

const SuperHero = "Superman";

console.log(SuperHero);
```

by export this in new file

```js
//index.js

require("./batman");
require("./superman");
```

the output will be

```
Batman
Superman

```

The same const variable naming conflict not affected in here because of module scoping

## MODULE WRAPPER

In Node.js each module is wrapped in a IIFE function before executed , this known as Module wrapper .
The purpose of Module wrapper is to provide a private scope for each module
It prevent variable and functions from leaking into global scope and conflicting with other modules

when we create a module and try to execute internally its wrapped like this

```js
(function (exports, require, module, __filename, __dirname) {
  // Our code
});
```

1. **exports:** A reference to `module.exports` that allow us to export functions,objects or variables

2. **require:** A function to import modules or JSON files into the current modules

3. **module:** A reference to the current module , enables to replace `module.export` with custom object or function

4. ** \_\_filename:** The absolute path to the current module file.

5. **\_\_dirname:** The directory name of the current module file.

## MODULE CACHING

Module caching is an mechanism that help to improve performance by loaded modules in memory after they first required.

```js

//app.js

module.export = {
    number:10;
}

```

```js
//index.js

const exportedObj = require("./app");

console.log(exportedObj.number); // output 10

exportedObj.number += 1;

const exportedObj2 = require("./app");

console.log(exportedObj2.number); // output 11
```

although its a great thing for performance in certain cases we need to avoid module caching
for that we can return the `module.export` as a function so for each instance have separate memory

example:

```js

//app.js

module.export = function(){
   return{
    number:10;
}
}

```

```js
//index.js

const exportedObj = require("./app");

console.log(exportedObj.number); // output 10

exportedObj.number += 1;

const exportedObj2 = require("./app");

console.log(exportedObj2.number); // output 10
```

In this case, each instance has separate memory, so changes to `exportedObj` do not affect `exportedObj2`.

## ES MODULES

ES modules (ECMAScript modules) in Node.js represent a modern and standardized way to organize and manage code in JavaScript. They are designed to work seamlessly across browsers and server environments like Node.js

1. **import and export:**

ES modules use `import` to bring in dependencies and `export` to expose functions, objects, or values from a module.

example:

```js
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
```

```js
// main.js
import { add, subtract } from "./math.js";

console.log(add(2, 3)); // Output: 5
console.log(subtract(5, 3)); // Output: 2
```

2. **Default Exports:**

You can have one default export per module, which can be imported without using curly braces.

```js
// calculator.js
const calculator = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
};

export default calculator;
```

```js
// main.js
import calculator from "./calculator.js";

console.log(calculator.add(2, 3)); // Output: 5
console.log(calculator.subtract(5, 3)); // Output: 2
```

3. **Named Exports:**

ES modules allow you to export multiple values from a module, which can be selectively imported. but the same name as export

```js
// utils.js
export const PI = 3.14159;
export function circleArea(radius) {
  return PI * radius * radius;
}
```

```js
// main.js
import { PI, circleArea } from "./utils.js";

console.log(PI); // Output: 3.14159
console.log(circleArea(5)); // Output: 78.53975
```

### IMPORTING JSON in NODE

While Exporting JSON (JavaScript Object Notation) file in Node.js Node automatically parse JSON file into javascript Object , so we can access this using object properties

```js
//data.json
{
   "name":"Aswanth",
    "age":26
}

```

```js
//index.json

const data = require("./data.json");

console.log(data);

// output will because

// {
//  name:"Aswanth",
//   age:26
// }
```

while importing json file always use `.json` notation , because if there a `js` file with same name in the folder the nod will give precedence to `.js` file .

## BUILT IN MODULES

Built-in Modules are modules Node.js ships with . It also referred as Core modules .
You need to import built-in modules before use

There are many Built-in modules but these are the 5 essential

1. path
2. events
3. fs
4. stream
5. http

### Path Modules

Path module provides utilities for working with fie and directory paths
To use the path module, you need to require item

```js
const path = require("node:path");
```

**node:Protocol**

Even though it is not necessary to use `node` specifier while import Built-in modules , but it considered as a good practice  
Returns the extension of the file in the path (e.g., .txt, .js).
Example:
js
￼Copy code
const filePath = '/user/local/bin/file.txt';
console.log(path.extname(filePath)); // Output: '.txt'
path.join([...paths]):

Joins multiple path segments into one single path. It handles and removes unnecessary or redundant slashes.
Example:
js
￼Copy code
const fullPath = path.join('/user', 'local', 'bin', 'file.txt');
console.log(fullPath); // Output: '/user/local/bin/file.txt'
path.resolve([...paths]):

Resolves a sequence of paths into an absolute path. It processes the paths from right to left, appending each to the preceding ones until an absolute path is generated.
Example:
js
￼Copy code
const absolutePath = path.resolve('user', 'local', 'bin');
console.log(absolutePath); // Output: '/current/working/directory/user/local/bin'
path.isAbsolute(path):

Returns true if the given path is an absolute path, and false if it is a relative path.
Example:
js
￼Copy code
console.log(path.isAbsolute('/user/local/bin')); // Output: true
console.log(path.isAbsolute('local/bin')); // Output: false
path.parse(path):

Returns an object representing significant components of the path (root, dir, base, ext, and name).
Example:
js
￼Copy code
const parsedPath = path.parse('/user/local/bin/file.txt');
console.log(parsedPath);
/_
Output:
{
root: '/',
dir: '/user/local/bin',
base: 'file.txt',
ext: '.txt',
name: 'file'
}
_/
path.format(pathObject):

Returns a path string from an object generated by path.parse().
Example:
js
￼Copy code
const pathObject = {
root: '/',
dir: '/user/local/bin',
base: 'file.txt',
ext: '.txt',
name: 'file'
};
const formattedPath = path.format(pathObject);
console.log(formattedPath); // Output: '/user/local/bin/file.txt'
Corrections:
Method Examples: In the examples for path.dirname, path.extname, and path.basename, make sure the example matches the method being described.
Typographical Errors: Corrected typos such as "fie" to "file," "item" to "it," and "necessary" to "necessary."
Overall, your documentation is well-written. These adjustments will ensure clarity and accuracy.

￼
￼
￼
￼
￼
￼
￼
￼￼

Its Identify clearly that the module you passed is a Built-in module
Also avoid future conflicts with Local module Naming

#### Methods of Path Modules

1. **path.basename(path):**
   Returns the last portion of a path (the file name or directory name).
   Example:

```js
const filePath = "/user/local/bin/file.txt";
console.log(path.basename(filePath)); // Output: 'file.txt'
```

2. **path.dirname(path)**
   Returns the directory name of a path, excluding the file name.

```js
const filePath = "/user/local/bin/file.txt";
console.log(path.dirname(filePath)); // Output: '/user/local/bin'
```

3. **path.extname(path):**
   Returns the extension of the file in the path (e.g.,` .txt`, `.js`).

```js
const filePath = "/user/local/bin/file.txt";
console.log(path.extname(filePath)); // Output: '.txt'
```

4. **path.join([...paths]):**
   Joins multiple path segments into one single path. It handles and removes unnecessary or redundant slashes.

```js
const fullPath = path.join("/user", "local", "bin", "file.txt");
console.log(fullPath); // Output: '/user/local/bin/file.txt'
```

5. **path.resolve([...paths]):**
   Resolves a sequence of paths into an absolute path. It works by processing the paths from right to left, appending each to the preceding ones until an absolute path is generated.

```js
const absolutePath = path.resolve("user", "local", "bin");
console.log(absolutePath); // Output: '/current/working/directory/user/local/bin'
```

6. **path.isAbsolute(path):**
   Returns `true` if the given path is an absolute path, and false if it's a relative path.

```js
console.log(path.isAbsolute("/user/local/bin")); // Output: true
console.log(path.isAbsolute("local/bin")); // Output: false
```

7. **path.parse(path):**
   Returns an object representing significant components of the path (`root`, `dir`, `base`, `ext`, and name).

```js
const parsedPath = path.parse("/user/local/bin/file.txt");
console.log(parsedPath);
/*
Output:
{
  root: '/',
  dir: '/user/local/bin',
  base: 'file.txt',
  ext: '.txt',
  name: 'file'
}
*/
```

8. **path.format(pathObject):**
   Returns a path string from an object generated by path.parse().

```js
const pathObject = {
  root: "/",
  dir: "/user/local/bin",
  base: "file.txt",
  ext: ".txt",
  name: "file",
};
const formattedPath = path.format(pathObject);
console.log(formattedPath); // Output: '/user/local/bin/file.txt'
```

### EVENTS MODULE

The events modules allow us to work with events in Node.js
An event is an action or an occurrence that happened in our application that we can respond to
Using events module , we can dispatch our own custom events and respond to those custom events in a non blocking manner

#### Event Emitter

Event module returns a class called EventEmitter which encapsulate functionality to emit events and respond to events Event emitter is an object

We can register event listeners to the `emitter` object using `on` method.

Using `emit` method in `emitter` object we can call the listeners with provided arguments.

there are many method we can attach to `emitter` object.

```js
const EventEmitter = require("node:events");

// Create an instance of EventEmitter
const emitter = new EventEmitter();

// Define an event listener for the 'greet' event
emitter.on("greet", (name) => {
  console.log(`Hello, ${name}!`);
});

// Emit the 'greet' event
emitter.emit("greet", "Aswanth");

// Output: Hello, Aswanth!
```

#### Major Methods of EventEmitter

1. **emitter.on(eventName, listener):**
   Adds a listener function that will be called whenever the specified event is emitted.

```js
emitter.on("data", (data) => {
  console.log(`Received data: ${data}`);
});
```

2. **emitter.emit(eventName, [...args]):**
   Emits an event, causing all listeners attached to that event to be called with the provided arguments.

```js
emitter.emit("data", "This is some data");
```

3. **emitter.once(eventName, listener):**
   Adds a one-time listener function for the specified event. The listener is invoked the first time the event is emitted and then removed.

```js
emitter.once("connection", () => {
  console.log("This will be logged only once.");
});
```

4. **emitter.removeListener(eventName, listener):**
   Removes a listener that was previously registered for the specified event.

```js
const callback = () => console.log("This should not be called");
emitter.on("removeTest", callback);
emitter.removeListener("removeTest", callback);
emitter.emit("removeTest"); // No output, as the listener was removed
```

5. **emitter.removeAllListeners([eventName]):**
   Removes all listeners, or those of the specified event.

```js
emitter.removeAllListeners("data");
```

6. **emitter.setMaxListeners(n):**
   By default, EventEmitter will print a warning if more than 10 listeners are added for a particular event. This method can be used to increase or decrease this limit.

```js
emitter.setMaxListeners(20);
```

7. **emitter.getMaxListeners():**
   Returns the current maximum listener limit.

```js
console.log(emitter.getMaxListeners()); // Output: 20 (after using setMaxListeners(20))
```

8. **emitter.listenerCount(eventName):**
   Returns the number of listeners listening to the specified event.

```js
console.log(emitter.listenerCount("greet")); // Output: 1 (as one listener was added)
```



