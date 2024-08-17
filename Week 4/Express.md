# EXPRESS
Express is a lightweight Node.js framework that provides robust set of features for web and mobile application

It provides  set of features that streamline the development of server-side applications, making it easier for developers to manage routing, middleware, and HTTP requests.

```js

const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})

```

### PORT
In Express.js, you can set up the port your application will run on using environment variables. Environment variables allow you to configure your application based on different environments (development, production, etc.) without hardcoding values directly into your code. This practice makes your application more flexible and secure.

```js
const express = require('express');
const app = express();

// Set the port using environment variables or a default value
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});

```

## HTTP Methods in Express.js
HTTP methods define the type of operation to be performed on a given resource when making a request to a server. Here are the most common HTTP methods used in web development:

### GET

* **Purpose:** Retrieve data from the server.
* **Usage:** When a client (usually a browser) wants to fetch data or a resource from the server without making any changes to the data on the server.

**Example:** A user visits a webpage or requests a list of items from an API.

```js 

app.get('/users', (req, res) => {
    res.send('Fetching list of users');
});

```

### POST

* **Purpose:** Send data to the server to create or update a resource.
* **Usage:** When a client submits form data, uploads a file, or sends any other kind of  data to the server.

**Example:** A user submits a registration form.

```js

app.post('/users', (req, res) => {
    res.send('Creating a new user');
});

```

### PUT

* **Purpose:** Update an existing resource on the server.
* **Usage:** When a client needs to update data or replace an existing resource entirely.

**Example:** A user updates their profile information.

```js

app.put('/users/:id', (req, res) => {
    res.send(`Updating user with ID ${req.params.id}`);
});

```

### DELETE

* **Purpose:** Remove a resource from the server.
* **Usage:** When a client wants to delete data from the server.

**Example:** A user deletes their account.

```js
app.delete('/users/:id', (req, res) => {
    res.send(`Deleting user with ID ${req.params.id}`);
});

```


### PATCH

* **Purpose:** Apply partial modifications to a resource.
* **Usage:** When a client needs to update part of a resource, rather than replacing the entire resource.

**Example:** A user updates their email address but not the entire profile.

```js

app.patch('/users/:id', (req, res) => {
    res.send(`Partially updating user with ID ${req.params.id}`);
});

```

##HTTP STATUS CODES

HTTP status codes are standard responses sent by a server to indicate the result of a client's request. They are categorized into five classes, each representing a different type of response:

1. **1xx: Informational Responses**
These codes indicate that the request was received and is being processed.

2. **2xx: Success**
These codes indicate that the request was successfully received, understood, and accepted.

* **200 OK:** The request was successful, and the server returned the requested data.
* **201 Created:** The request was successful, and a new resource was created as a result.

3. **3xx: Redirection**
These codes indicate that further action is needed to complete the request.

* **301 Moved Permanently:** The requested resource has been permanently moved to a new URL.
* **302 Found:** The requested resource resides temporarily under a different URL.

4. **4xx: Client Errors**
These codes indicate that there was an error with the request made by the client.

* **400 Bad Request:** The server could not understand the request due to invalid syntax.
* **401 Unauthorized:** The client must authenticate itself to get the requested response.
* **403 Forbidden:** The client does not have access rights to the content.
* **404 Not Found:** The server could not find the requested resource.

5. **5xx: Server Errors**
These codes indicate that the server encountered an error and could not fulfill the request.

* **500 Internal Server Error:** The server encountered an unexpected condition that prevented it from fulfilling the request.

* **503 Service Unavailable:** The server is not ready to handle the request, often due to maintenance or overload.

### Sending Status Codes in Express
In Express, you can send HTTP status codes using the res.status() method. This method sets the HTTP status code for the response and can be chained with res.send() or other methods to send the response body.

Here’s an example of how to use it:

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.status(200).send('OK');
});

app.get('/not-found', (req, res) => {
    res.status(404).send('Not Found');
});

app.get('/error', (req, res) => {
    res.status(500).send('Internal Server Error');
});

app.listen(3000, () => {
    console.log('Server is running on port 3000');
});

```

## MIDDLEWARE

Middleware functions are the backbone of an Express.js application. They are functions that have access to the request object (`req`), the response object (`res`), and the next middleware function in the application’s request-response cycle. Middleware functions can perform a variety of tasks, such as:

* Executing code.
* Modifying the request and response objects.
* Ending the request-response cycle.
* Calling the next middleware function in the stack.

In an Express app, middleware functions are executed sequentially, forming a middleware stack. This stack allows you to organize and handle different aspects of a request, from parsing data to logging and error handling.

###Built-in Middleware
Express comes with several built-in middleware functions to handle common tasks:

* **`express.json()`:**
This middleware parses incoming requests with JSON payloads and is based on the body-parser library. It allows you to handle JSON data in the request body.

```js

const express = require('express');
const app = express();

app.use(express.json());

app.post('/data', (req, res) => {
    console.log(req.body); // Access parsed JSON data
    res.send('Data received');
});

app.listen(3000, () => {
    console.log('Server is running on port 3000');
});

```

* **`express.urlencoded():`**
This middleware parses incoming requests with URL-encoded payloads. It is also based on the body-parser library and is typically used for form submissions.

```js

app.use(express.urlencoded({ extended: true }));

```
The `extended: true`option allows for parsing complex objects, while `extended: false` supports simpler structures.

### Third-Party Middleware

Express also supports third-party middleware, which provides additional functionality that you can integrate into your application. Some popular third-party middleware includes:

* **`body-parser:`**
Even though `express.json()` and `express.urlencoded()` are built-in, body-parser was traditionally used for parsing incoming request bodies. It's still useful for more specialized parsing needs.

```js
 
const bodyParser = require('body-parser');
app.use(bodyParser.json());

```

* **`cookie-parser:`**
This middleware parses cookies attached to the client request object, making it easier to manage and use cookies.

```js 

cookie-parser:
This middleware parses cookies attached to the client request object, making it easier to manage and use cookies.

```

* **`morgan:`**
Morgan is a popular middleware for logging HTTP requests and responses. It provides various predefined formats or can be customized.

```js 

const morgan = require('morgan');
app.use(morgan('dev'));

```

* **`cors:`**
This middleware enables Cross-Origin Resource Sharing (CORS) in your Express app, allowing your API to handle requests from different origins.

```js 

const cors = require('cors');
app.use(cors());

```

### Custom Middleware
You can create your own middleware to perform specific tasks in your application. A custom middleware function is defined similarly to a route handler, but it includes a third argument, next, which is used to pass control to the next middleware function.

Example of a simple custom middleware:

```js 

const express = require('express');
const app = express();

// Custom middleware to log request details
const requestLogger = (req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next(); // Pass control to the next middleware
};

app.use(requestLogger);

app.get('/', (req, res) => {
    res.send('Home Page');
});

app.listen(3000, () => {
    console.log('Server is running on port 3000');
});


```
n this example, the `requestLogger` middleware logs the HTTP method and URL for each request before passing control to the next middleware or route handler.

## Static Files handling

In an Express.js application, serving static files (such as images, CSS files, and JavaScript files) is a common requirement. Static files are those that do not change dynamically and are served directly to the client. Express makes it easy to serve these files using the `express.static()` middleware.

```js 

const express = require('express');
const app = express();

// Serving static files from the "public" directory
app.use(express.static('public'));

app.get('/', (req, res) => {
    res.send('Home Page');
});

app.listen(3000, () => {
    console.log('Server is running on port 3000');
});

```

In this example:

* The express.static('public') middleware serves all files in the public directory. This means that if you have an index.html file in the public directory, it can be accessed via http://localhost:3000/index.html.

* If a request is made to a route that does not match any of the static files, Express will continue down the middleware stack, eventually reaching the route handlers.

You can also serve multiple directories or specify a virtual path prefix:

```js 
// Serving static files from "public" directory
app.use('/static', express.static('public'));

// Now the static files can be accessed via http://localhost:3000/static/filename.ext
```

## Cookies, Authentication, and Session Management in Express

### Cookies

Cookies are small pieces of data stored on the client’s browser, often used for tracking, storing user preferences, or managing sessions. In Express, you can easily manage cookies using the `cookie-parser` middleware.

Setting Up **cookie-parser:**

First, install the `cookie-parser`middleware:

```js 
npm install cookie-parser

```

Next, use it in your Express application:

```js 
const express = require('express');
const cookieParser = require('cookie-parser');
const app = express();

app.use(cookieParser());

```

**Setting a Cookie:**

To set a cookie, use the res.cookie() method:

```js 

app.get('/set-cookie', (req, res) => {
    res.cookie('username', 'Aswanth', { maxAge: 900000, httpOnly: true });
    res.send('Cookie has been set');
});

```

In this example:

* **'username', 'Aswanth':** Sets a cookie named username with the value `Aswanth`.
* **maxAge: 900000:** Sets the cookie to expire in 15 minutes.
* **httpOnly: true:** Ensures the cookie is only accessible via the web server, enhancing security.


**Reading a Cookie:**
You can access the cookies sent by the client using `req.cookies:`

```js 

app.get('/get-cookie', (req, res) => {
    const username = req.cookies.username;
    res.send(`Cookie retrieved: ${username}`);
});

```

**Deleting a Cookie:**
To delete a cookie, use `res.clearCookie():`

```js 
app.get('/clear-cookie', (req, res) => {
    res.clearCookie('username');
    res.send('Cookie has been cleared');
});

```

### Session Management

Sessions are used to maintain the state across multiple requests in a web application. In Express, session management is commonly handled using the `express-session` middleware.

Install the express-session package:

```js 
npm install express-session

```



## REST API
 
