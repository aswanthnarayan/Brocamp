

* React.strict mode and why loging 2 tim,es in console

# CSR vs SSR

CSR (Client-Side Rendering) and SSR (Server-Side Rendering) refer to two different approaches to rendering a web application.

## Client-Side Rendering (CSR)

Client-Side Rendering is a rendering method where the initial HTML sent by the server to the browser is minimal (usually just a shell or an empty div), and the JavaScript is responsible for rendering the entire UI in the browser after the page loads. React’s default behavior with tools like Create React App is CSR.

**How CSR Works:**

* The server sends a bare-bones HTML file with minimal content to the browser.
* The browser downloads the JavaScript bundle (containing React and the app logic).
* Once the JavaScript is fully loaded and executed, React takes over and renders the app dynamically in the browser.
* The user sees a blank page until the JavaScript is fully downloaded and executed, after which the UI becomes visible.

**Pros of CSR:** 

* **Rich client-side interactivity:** React (and other SPA frameworks) offer highly interactive UIs once the JavaScript is loaded.
* **Reduces server load:** The server only serves static files (HTML, CSS, JS), and the rendering is done entirely on the client.
* **Simpler development process:** CSR is simpler to implement with tools like Create React App, especially for Single Page Applications (SPA).

**Cons of CSR:**

* **Slower initial load time:** The page is often blank or shows a loading spinner until the JavaScript is fully loaded, leading to a poor First Contentful Paint (FCP) and Time to Interactive (TTI).
* **SEO limitations:** Since the content is rendered on the client-side, search engine crawlers may have difficulty indexing the site correctly, especially if they don’t fully execute JavaScript (though this has improved over time).
* **Slower on low-performance devices:** On slower devices or networks, rendering in the client can take time, leading to degraded user experience.

## Server-Side Rendering (SSR)

Server-Side Rendering is a technique where the server generates the full HTML content of the page and sends it to the browser, which can then be displayed immediately. In the case of React SSR, the React components are rendered into HTML on the server, and the HTML is sent to the client along with the necessary JavaScript to "hydrate" the page and make it interactive.

**How SSR Works:**

* The user requests a page.
* The server runs the React application on the server, renders the full page as HTML with all the required content.
* The fully rendered HTML page is sent to the browser, and the user sees the page almost immediately.
* Once the HTML is loaded, React’s JavaScript bundle is downloaded, and the page is hydrated (i.e., React takes over the static HTML and makes it interactive by attaching event handlers).

**Pros of SSR:**

* **Faster initial load time:** Since the server sends fully rendered HTML, users see the content quickly, improving the First Contentful Paint (FCP).
* **Better for SEO:** Search engine crawlers can easily index the fully-rendered HTML content, improving SEO for public-facing websites.
* **Improved performance for slower devices:** The rendering burden is shifted to the server, so even low-performance devices can display the initial content quickly.

**Cons of SSR:** 

* **Increased server load:** The server has to generate and render the HTML for each request, which can lead to higher server costs and slower performance under high load.
* **Slower page transitions:** For SPAs, moving from page to page can be slower compared to CSR, as each new page requires a new server request and render.
* **Complexity:** SSR setups are generally more complex to implement and require additional tooling (e.g., Next.js for React SSR).

### Hydration in SSR

Hydration is a crucial concept in SSR. When the server sends the fully-rendered HTML to the browser, the page is static at first. After the JavaScript bundle loads, React hydrates the page by attaching event listeners and making it interactive.

**How Hydration Works:** 

* **Initial SSR:** The server sends pre-rendered HTML to the browser.
* **Client-side Hydration:** React's JavaScript bundle is downloaded and executed, and React reuses the existing HTML to attach event listeners and other dynamic behavior.
* This process allows for faster first content paint, while still maintaining interactivity.


# REACT 

React is a JavaScript library developed by Facebook for building user interfaces. It lets you build components that can be reused and combined to create entire pages or applications.
The key benefit of React is that it allows for efficient rendering of complex and changing data.
This is achieved through a virtual DOM that minimizes direct manipulation of the page, resulting in better performance.
React is widely used in web development due to its scalability, simplicity, and extensive community support.

# Imperative and Declarative programming

## Imperative programming

 Imperative programming is about specifying the exact steps or commands the program should execute to achieve the desired result. It's more about the implementation details and less about the problem description. Examples of imperative languages include C, Java, and Python

 React's `createElement` method is an example of a more imperative approach since you have to define each element creation explicitly.

```jsx
import React from 'react';

const MyComponent = () => {
  return React.createElement(
    'div',
    null,
    React.createElement('h1', null, 'Hello, World!')
  );
};

export default MyComponent;
```
*In this example, you explicitly tell React to create a div element and a h1 element inside it. This is more imperative since you are specifying the exact steps to create the elements, rather than simply describing what the UI should look like*


## Declarative programming

In declarative programming, you describe what you want to achieve without specifying how the program should achieve the result. This approach focuses on what the problem is and what the solution should look like, rather than the specific steps needed to reach that solution. SQL and HTML are examples of declarative languages.

React is inherently declarative, meaning you describe what the UI should look like based on the current state, and React updates the DOM accordingly.

```jsx 
import React from 'react';

const MyComponent = () => {
  return (
    <div>
      <h1>Hello, World!</h1>
    </div>
  );
};

export default MyComponent;
```
*In this example, you are describing what the UI should look like using JSX, and React will take care of creating the actual DOM nodes under the hood. You are not specifying how to create the div and h1, but rather what they should be* 


# DOM 

The **DOM (Document Object Model)** is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style, and content. Essentially, it is an in-memory representation of the HTML structure of the web page.

When you interact with elements in a web page (such as clicking buttons, filling out forms, etc.), the browser modifies the DOM, which can lead to updates in the UI.

However, directly manipulating the DOM is slow and inefficient because browsers have to repaint and reflow the page on every change, which can lead to performance bottlenecks, especially with complex UIs.

# VIRTUAL DOM 

The **Virtual DOM (VDOM)** is an abstraction of the actual DOM, created and maintained in memory by React. Instead of interacting with the real DOM directly, React works with the Virtual DOM, which allows it to optimize the rendering process.

* It's a lightweight JavaScript object that represents the actual DOM
* Whenever the state of a component changes, React creates a new virtual DOM, which is then compared with the previous one using a process called reconciliation.
* After comparing the two virtual DOMs, React calculates the minimal set of changes required to update the real DOM efficiently, thus minimizing direct manipulations to the actual DOM.

## Virtual DOM - working

1. ### Component Rendering

* When the state or props of a component changes, React triggers a re-render of the component
* React generates a new Virtual DOM tree to reflect the updated state of the application.

2. ### Diffing Algorithm

React uses a diffing algorithm to compare the new Virtual DOM tree with the previous one (this happens in memory).

**Element-level Diffing**

React compares two elements at the same level. If they are of different types (e.g., <div> vs <span>), React destroys the old element and creates a new one.
If they are of the same type, React will keep the same underlying DOM node and update only the changed properties.

**Component-level Diffing**

For custom components, if the component type is the same (same class or function component), React updates the component's props and state. If the component type is different, React unmounts the old component and mounts the new one.

3. ### Reconciliation

* Once React identifies the differences, it generates a list of updates needed to bring the real DOM in sync with the new Virtual DOM.
* React updates only the necessary parts of the real DOM, avoiding full re-renders and improving performance.

4. ### Minimal DOM Manipulation 

* After reconciliation, React sends the minimal changes to the browser’s actual DOM.

* By manipulating only the parts of the DOM that have changed, React ensures a more efficient and faster UI update process compared to directly interacting with the real DOM.

## Batching in React

Batching refers to React's ability to group multiple state updates into a single re-render, rather than re-rendering the component for each state update. This improves performance by minimizing the number of updates to the DOM.

* React reduces unnecessary renders by processing state updates in bulk.
* Fewer renders improve performance, especially in complex applications.

* In synchronous updates, React automatically batches multiple state updates triggered inside event handlers into a single update.

```jsx
function handleClick() {
  setCount(count + 1);
  setMessage("Clicked");
  // Both updates will be batched and React will only re-render once.
}
```

* In React 18 and beyond, React also supports automatic batching for asynchronous updates such as promises, timeouts, or event loops. This means multiple updates in asynchronous code will also be grouped together and processed in a single re-render.

```jsx
setTimeout(() => {
  setCount(count + 1);
  setMessage("Updated"); 
  // In React 18, both updates will be batched even though they are inside a timeout.
}, 1000);
```

| **Aspect**       | **DOM (Document Object Model)**                                                                         | **Virtual DOM (VDOM)**                                                                                     |
|------------------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Definition**   | A programming interface for web documents that represents the structure of the page.                    | A lightweight in-memory representation of the actual DOM in React.                                         |
| **Modification** | Directly manipulated by JavaScript, which causes immediate changes in the UI.                           | React manipulates the Virtual DOM, then reconciles it with the real DOM.                                   |
| **Performance**  | Slow, because each change leads to recalculations (reflow/repaint) of the UI in the browser.            | Faster, as multiple changes are applied in a batch to the real DOM after comparison (diffing).             |
| **Efficiency**   | Inefficient for frequent updates, as every change affects the entire DOM structure, causing re-renders. | Highly efficient, only the minimal set of changes are applied to the real DOM after the diffing algorithm. |
| **Updates**      | Changes applied directly and immediately affect the visible UI.                                         | Changes first applied to the Virtual DOM and then selectively updated in the real DOM.                     |
| **Rendering**    | Triggers a re-render every time the DOM is updated, leading to performance issues with large UIs.       | Minimizes re-rendering by calculating and applying only the necessary changes.                             |
| **Batching**     | No built-in batching mechanism, each DOM update happens individually.                                   | React batches multiple updates together to reduce real DOM manipulations.                                  |
| **Reactivity**   | No concept of state-driven reactivity; manual manipulation is needed.                                   | React updates the Virtual DOM based on changes in state or props, and re-renders accordingly.              |
| **Memory Usage** | Direct memory usage for the actual DOM tree and browser rendering processes.                            | Lightweight representation in memory, only synced with the real DOM when necessary.                        |



# JSX 

**JSX (JavaScript XML)** is a syntax extension for JavaScript that allows you to write HTML-like code within your JavaScript files. JSX provides a declarative way to define UI components by blending JavaScript with HTML-like tags

* Every JSX expression must have one root element. This means that any component you return must have a single parent element wrapping all other child elements.
-- This is required because JSX gets compiled to React.createElement(), which can only return one root element.

```jsx
function MyComponent() {
  return (
    <div>   {/* Single root element */}
      <h1>Hello, World!</h1>
      <p>This is a paragraph.</p>
    </div>
  );
}
```
* If you attempt to return two sibling elements without a single root element, you will get an error

* To avoid this, you can use a wrapper element (like `<div>` or `<Fragment>`) to ensure there is one root element

## FRAGMENTS 

In React, a fragment is a lightweight way to group a list of children without adding extra nodes to the DOM. Fragments were introduced to solve the problem of having to wrap list items in an extra DOM node (like a `<div> `or `<span>`) when returning multiple elements from a component

`<Fragment>`, often used via `<>...</>`

```jsx
<>
  <OneChild />
  <AnotherChild />
</>
```
##

JSX is not HTML; it’s just syntactic sugar for React.createElement() calls. This means JSX gets transpiled to JavaScript that looks something like this

```jsx
// JSX
const element = <h1>Hello, world!</h1>;

// Transpiled JavaScript
const element = React.createElement('h1', null, 'Hello, world!');
```
*Since JSX is JavaScript, you can use any JavaScript logic, such as conditionals, loops, etc., within JSX expressions, giving you great flexibility.*

## Passing JavaScript Expressions in JSX

JSX allows you to pass JavaScript expressions directly into your markup by using curly braces `{}`.

1. **Variables:**

You can pass variables into JSX using curly braces {}.

```jsx
const name = "John";

function MyComponent() {
  return <h1>Hello, {name}!</h1>;  // Output: Hello, John!
}
```

2. **Expressions:** 

You can perform calculations or other expressions inside JSX

```jsx
const num1 = 5;
const num2 = 10;

function MyComponent() {
  return <p>{num1 + num2}</p>;  // Output: 15
}
```

3. **Ternary Operators:**

Conditional rendering can be done using the ternary operator.

```jsx 
const isLoggedIn = true;

function MyComponent() {
  return (
    <div>
      {isLoggedIn ? <h1>Welcome Back!</h1> : <h1>Please Log In</h1>}
    </div>
  );
}
```
4. **Function Calls:** 

You can call functions directly within JSX using curly braces {}.

```jsx
function greetUser(name) {
  return `Hello, ${name}!`;
}

function MyComponent() {
  return <h1>{greetUser('Alice')}</h1>;  // Output: Hello, Alice!
}
```

5. **Passing Functions as Props:** 

JSX also allows you to pass functions as props to child components.

```jsx
function ChildComponent({ onClick }) {
  return <button onClick={onClick}>Click Me</button>;
}

function ParentComponent() {
  const handleClick = () => {
    alert('Button clicked!');
  };
  
  return <ChildComponent onClick={handleClick} />;
}
```
6. **Arrays:** 

You can pass arrays in JSX. For example, rendering a list of items

```jsx
const items = ['Apple', 'Banana', 'Cherry'];

function MyComponent() {
  return (
    <ul>
      {items.map(item => <li key={item}>{item}</li>)}  {/* Output: List of fruits */}
    </ul>
  );
}
```
7. **Objects:** 

You can also pass object properties dynamically in JSX:

```jsx
const user = { firstName: 'Jane', lastName: 'Doe' };

function MyComponent() {
  return <h1>Hello, {user.firstName} {user.lastName}!</h1>;  // Output: Hello, Jane Doe!
}
```

# PROPS

In React, props (short for "properties") are a mechanism used to pass data from one component to another, typically from a **parent component** to a **child component**.

* Props are inputs to a component that are passed as attributes in JSX. They allow React components to be reused with different data 
* Props are read-only (immutable), which means that a child component cannot modify the props it receives
* Components can be stateless if they only rely on props and don’t need their own internal state.

ex:

```jsx
// Parent Component
const ParentComponent = () => {
  return (
    <div>
      <ChildComponent message="Hello from Parent!" />
    </div>
  );
};

// Child Component
const ChildComponent = (props) => {
  return <h1>{props.message}</h1>;  // Output: Hello from Parent!
};
```

## Destructuring Props

To avoid repeating props.someProperty multiple times in your component, you can destructure the props object. This is a common pattern, especially when passing multiple props

```jsx
const Greeting = ({ name, age }) => {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Your age is {age}.</p>
    </div>
  );
};
```
*Now, name and age can be used directly instead of having to reference `props.name` and `props.age`.*

## Props with Default Values
You can assign default values to props in case the parent component doesn't pass them down. This is helpful when you want to ensure that a component has some default behavior or values if no props are provided.

1. **Using Default Parameters in Functional Components:**

```jsx

const Greeting = ({ name = "Guest" }) => {
  return <h1>Hello, {name}!</h1>;
};

// If no name is passed, it will render "Hello, Guest!"

```

2. **Using defaultProps:**

Another way is to define defaultProps for the component, which is especially common in class components but also works with functional components

```jsx
const Greeting = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};

// Set default props
Greeting.defaultProps = {
  name: "Guest",
};
```

## Passing Functions as Props (Callback Functions)

In React, you can also pass functions as props. This is useful for callback functions, where a child component can communicate back to its parent.

```jsx 
const ParentComponent = () => {
  const handleClick = () => {
    alert("Button clicked!");
  };

  return <ChildComponent onClick={handleClick} />;
};

const ChildComponent = ({ onClick }) => {
  return <button onClick={onClick}>Click Me</button>;
};
```
in here,

* The ParentComponent passes a function handleClick as a prop called onClick to ChildComponent.
* When the button in ChildComponent is clicked, it calls the onClick function, which triggers the alert in the parent.

*This pattern is commonly used for event handling (like button clicks, form submissions, etc.) and is fundamental for parent-child communication in React*.

## Prop Types and Validation
In larger applications, it is a good practice to validate the props your components receive. This helps ensure that a component is used correctly and with the right data types. React provides a built-in mechanism for prop validation using **prop-types**.

```jsx
import PropTypes from 'prop-types';

const Greeting = ({ name, age }) => {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>You are {age} years old.</p>
    </div>
  );
};

// Validate props
Greeting.propTypes = {
  name: PropTypes.string.isRequired,  // name must be a string and is required
  age: PropTypes.number,              // age must be a number (optional)
};

// Set default props
Greeting.defaultProps = {
  age: 18,  // Default age is 18 if not provided
};
```

In this example:

* name must be a string, and it’s required (if not passed, React will issue a warning).
* age must be a number, but it’s optional.
* If no age is passed, Greeting will default to 18.

##  Children Prop
The `children` prop is a special prop that can be used to pass nested content into a component. It is often used to create wrapper components that can contain arbitrary JSX as children

```jsx
const Wrapper = ({ children }) => {
  return <div className="wrapper">{children}</div>;
};

const App = () => {
  return (
    <Wrapper>
      <h1>This is the title</h1>
      <p>This is some content inside the wrapper.</p>
    </Wrapper>
  );
};
```

In this example:

* The `Wrapper` component accepts any JSX content as `children` and wraps it in a div.
* The `App` component passes an `<h1>` and a `<p>` as the children of `Wrapper`.

# STATES

In React, state refers to an object that holds dynamic data or information about a component that can change over time

* **State is local:** It is owned and managed by a component and is not accessible to other components directly
* **State causes re-rendering:** When the state of a component is updated, React re-renders the component, ensuring that the UI is in sync with the current state
* **State is mutable:** Unlike props, which are read-only, state can be changed within the component

## Declaring State in React - Functional Components

With the introduction of **React Hooks** in React 16.8, functional components gained the ability to use state via the useState hook. This is now the recommended way of managing state in most modern React applications

```jsx 
import React, { useState } from 'react';

const MyComponent = () => {
  // Declare a state variable "count" with initial value 0
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      {/* Update state using setCount */}
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```

* **useState()**: This is a hook that allows you to declare a state variable in a functional component.
* The first value `(count)` is the current state.
* The second value `(setCount)` is the function used to update the state

## Updating State in React

React batches multiple state updates together to optimize performance. As a result, state updates might not be immediately reflected after calling `useState` setter function

```jsx
setCount(prevCount => prevCount + 1);
```

Using useState, state is replaced rather than merged. If you are managing an object in state, you need to manually merge the old state with the new state to avoid overwriting unrelated properties

```jsx
const [user, setUser] = useState({ name: 'John', age: 25 });

// To update just the age:
setUser((prevUser) => ({
  ...prevUser,
  age: 26
}));
```

# PROPS VS STATE

|   **Aspect**   |                         **Props**                         |                   **State**                   |
|:--------------:|:---------------------------------------------------------:|:---------------------------------------------:|
| **Mutability** | Immutable: Cannot be modified by the receiving component. | Mutable: Can be updated within the component. |
|  **Ownership** | Passed from parent to child components.                   | Local to the component that owns it.          |
|   **Purpose**  | Used to pass data and functions to child components.      | Used to manage internal data in a component.  |
| **Updates**    | Does not cause re-renders in the parent component.        | Causes re-renders when state is updated.      |

# COMPONENTS

Component is a reusable, modular piece of code that represents a part of a user interface. Components can accept input (props) and return a React element that describes how a section of the UI should appear. Components help break down complex UIs into smaller, maintainable, and reusable pieces

## Component Reusability

One of the key benefits of React components is reusability. React components can be built in a modular way so that they can be reused across the application. Reusability helps you avoid repetition, simplifies maintenance, and promotes consistency.

**To Achieve Reusability:**

* **Modular Design**: 

Break your UI into small, reusable components. For example, a button or input field can be a reusable component, rather than defining them multiple times in different places

```jsx
const Button = ({ label, onClick }) => {
  return <button onClick={onClick}>{label}</button>;
};
```

* **Props for Customization:**

Props allow components to be flexible. You can pass different data or event handlers to the same component to make it behave differently.

```jsx
<Button label="Submit" onClick={handleSubmit} />
<Button label="Cancel" onClick={handleCancel} />
```

## Abstraction of Components

Abstraction in React components means Instead of having a complex component that does everything, you break it down into smaller, more specialized components.

This approach will help you to achieve

1. **Maintainability:** Smaller, abstracted components are easier to maintain and test.
2. **Separation of Concerns:** By abstracting concerns into different components, you avoid mixing logic (e.g., UI and business logic) in one component.
3. **Readability:** Smaller components tend to be more readable and understandable.

```jsx
const LoginForm = () => {
  return (
    <form>
      <UsernameInput />
      <PasswordInput />
      <SubmitButton />
    </form>
  );
};

const UsernameInput = () => <input type="text" placeholder="Username" />;
const PasswordInput = () => <input type="password" placeholder="Password" />;
const SubmitButton = () => <button type="submit">Login</button>;
```

## Composability of Components

Composability refers to the ability to build complex UIs by combining smaller, reusable components. React encourages you to think in terms of components that can be composed together to create the desired UI.

**Composing Components:**

You can compose multiple components to build larger, more complex structures. Components in React can be nested inside each other to create flexible and scalable UIs.

```jsx
const Header = () => <header><h1>My App</h1></header>;
const Footer = () => <footer>Footer content</footer>;

const Layout = ({ children }) => {
  return (
    <div>
      <Header />
      <main>{children}</main>
      <Footer />
    </div>
  );
};

const App = () => {
  return (
    <Layout>
      <p>Welcome to my application!</p>
    </Layout>
  );
};
```

In this example, `Header`, `Footer`, and `Layout` are composed together to form a reusable layout structure. The `Layout` component accepts `children`, allowing us to pass content into the layout dynamically. This is how composability allows you to create flexible UIs.


## Higher-Order Component (HOC)

A Higher-Order Component is a function that:

1. Takes a component as an argument (often referred to as a "wrapped component").
2. Returns a new, enhanced component by adding additional props, state, or behavior.

Think of HOCs as wrappers that add new capabilities to existing components without modifying them directly.

### Why Use Higher-Order Components?

* **Reusability:** HOCs allow you to reuse logic across multiple components. If you have multiple components that need the same logic (e.g., authentication checks, fetching data, or modifying props), HOCs allow you to encapsulate that logic in one place and apply it to many components.

* **Separation of Concerns:** HOCs promote a clean separation of concerns by moving logic out of components, thus keeping them focused on rendering and UI concerns while the HOC handles logic like data fetching or handling subscriptions.

* **Code Organization:** HOCs can help in abstracting common logic (like side effects) from multiple components into a single, reusable HOC.

```jsx
import React from 'react';

// A simple Higher-Order Component
const withLogger = (WrappedComponent) => {
  return (props) => {
    console.log('Props passed to WrappedComponent:', props);
    return <WrappedComponent {...props} />;
  };
};

// Example component that will use the HOC
const MyComponent = ({ message }) => {
  return <h1>{message}</h1>;
};

// Apply the HOC
const EnhancedComponent = withLogger(MyComponent);

// Usage
function App() {
  return <EnhancedComponent message="Hello, World!" />;
}

export default App;
```
**How this works:**

* `withLogger` is the HOC that wraps around `MyComponent` and logs its props.
* The new component `EnhancedComponent` behaves like `MyComponent` but with additional logging behavior.

# KEYS

* For lists or collections, React uses keys to identify elements uniquely. This helps React optimize reordering and avoid unnecessary re-renders

* Without keys, React will simply re-render every item in the list, which can be inefficient. By providing unique keys, React can track changes to individual elements (such as adding, removing, or reordering items).

```jsx
import React from 'react';

const FruitList = () => {
  const fruits = ['Apple', 'Banana', 'Orange', 'Mango'];

  return (
    <ul>
      {fruits.map((fruit, index) => (
        // Always provide a unique key to help React optimize re-renders
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
};

export default FruitList;
```

# React.StrictMode

`React.StrictMode` is a development mode feature in React that helps identify potential problems in your application by highlighting potential issues early on. It does not affect the production build but assists developers in writing more robust and future-proof code during the development phase

**USE CASE**

1. **Detects Side Effects in useEffect**

`Strict Mode` renders components twice during development (in non-concurrent mode) to help detect **side effects** that are not properly cleaned up

This double rendering happens:

* **When mounting:** It ensures that any side effects are cleaned up properly.
* **When unmounting and re-rendering:** React verifies that the cleanup logic is correct in components using the useEffect hook.

```jsx
useEffect(() => {
  console.log('Effect is running');
  return () => {
    console.log('Cleaning up the effect');
  };
}, []);
```

```jsx 
//output will be:

// Effect is running
// Cleaning up the effect
// Effect is running
```

*With Strict Mode, you’ll see this effect log twice to help detect potential issues with the cleanup process.*

2.  **Warns About Deprecated Features**

Strict Mode will warn you about the usage of deprecated React features or patterns that might be removed in future versions of React. This ensures that your app remains up-to-date with the latest React standards and best practices.


# 