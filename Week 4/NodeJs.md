# Node.js

Node.js is an open-source, cross-platform JavaScript runtime environment that allows you to build end-to-end JavaScript applications.

With Node.js, you can run JavaScript code outside the browser. However, in the Node.js environment, you don't have access to Web APIs like fetch or setTimeout.

Instead, Node.js provides lower-level computing capabilities, such as accessing the file system, through C++ bindings using the V8 engine. Node.js consists of C++ files that form the core features, and JavaScript files that expose some common utilities and C++ features for easier use.

![NodeJS Runtime](https://raw.githubusercontent.com/aswanthnarayan/Brocamp/main/Daigrams/NodeJS_Runtime.png)


## JavaScript Engine

A JavaScript engine converts JavaScript code into machine code, enabling the computer to perform specific tasks.

## JavaScript Runtime

A JavaScript runtime is an environment that provides all the necessary components to run a JavaScript program. Every browser has a JavaScript engine, which is a key component of the JavaScript runtime.

## Browser vs. Node.js

In the browser, most of the time, we interact with the DOM or other Web Platform APIs like Cookies. In contrast, Node.js provides APIs through its modules, such as filesystem access, which are not available in the browser.

