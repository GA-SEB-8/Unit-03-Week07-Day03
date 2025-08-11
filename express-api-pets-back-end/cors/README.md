<h1>
  <span class="headline">Pets Back-End</span>
  <span class="subhead">CORS</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to implement Cross-Origin Resource Sharing (CORS) in an Express application to allow the API to be accessed by other domains.

## CORS

We have created a RESTful API, but we still have one more thing to do before we can call it complete. We need to handle [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) to allow our API to be accessed by other domains. But first, what is CORS and why do we need it?

Let's journey back in time when the internet was young. Websites were static and the only way to interact with them was to click on links. But as the internet grew, so did the need for more dynamic and interactive websites. This led to the creation of the XMLHttpRequest object, which allowed websites to make requests to servers without having to reload the page. This was the birth of AJAX (Asynchronous JavaScript and XML), and it was the beginning of the modern web.

As AJAX became popular, allowing web pages to request data asynchronously, there was a need for guidelines on how websites interact with each other. This led to the development of CORS, or Cross-Origin Resource Sharing. CORS is a security mechanism that controls how web applications on one domain can interact with resources located on another domain. It provides a set of rules that servers can apply to either allow or deny access to their resources based on the origin of the request.

### Real-World Example: Amazon

Imagine a massive platform like Amazon, which has a backend supporting numerous endpoints, much larger than the simple applications we build. Amazon is accessed through various methods such as its website, mobile apps, Alexa app, Echo devices, and Fire Sticks. Each access method might come from different domains.

### Why CORS matters

By default, browsers block HTTP requests across different domains for security reasons. This means, for example, if your web application tries to request data from a domain other than its own, the browser will deny that request to protect against potential security threats.

CORS provides a way to bypass this restriction. It allows servers to specify which domains are permitted to access their resources. This is managed by adding a special header to the responses sent from the server to the client, known as `Access-Control-Allow-Origin`. This header tells the browser which domains are allowed to interact with the server, ensuring that only authorized domains can make requests.

## Implement CORS in Express

First we need to install the `cors` package. This package will allow us to easily implement CORS in our Express application.

```bash
npm install cors
```

The way we implement CORS in our application is by adding it as a middleware to our server file:

```js
// server.js
// Other imports above
const cors = require('cors');

const petRouter = require('./controllers/pets.js');

app.use(cors());
// All other server code below
```

If we leave the `cors()` function empty, it will allow ***all domains*** to access our resources. This is fine for development, but in production we will want to specify which domains are allowed to access our resources. We can do this by passing an options object to the `cors()` function. This object will have a `origin` key with a value of the domain we want to allow. We can also pass in an array of domains to allow multiple domains to access our resources.

```js
// server.js
// Other imports above
const cors = require('cors');

const petRouter = require('./controllers/pets.js');

app.use(cors({ origin: 'http://localhost:5173' }));
// All other server code below
```

> 🧠 The `http://localhost:5173` is the most common port used by Vite for React projects, but this can be changed to whatever we need to whitelist.
