<h1>
  <span class="headline">Fetching Data in React</span>
  <span class="subhead">Fetching Data with Service Functions</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create service functions that make fetch requests in a React app.

## The Fetch API

Let's get some practice using AJAX techniques in React. While there are several options for retrieving external data in React, we'll use the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

The Fetch API is a tool provided by modern browsers (and Node.js) that allows developers to make asynchronous HTTP requests using JavaScript. With the Fetch API's `fetch()` method, our React apps can communicate with servers hosted elsewhere on the web.

In this lesson, we'll use the `fetch()` method to retrieve weather information from the [Weather API](https://www.weatherapi.com/). When we make a request to the Weather API server, we can expect JSON data as a response. This data can then be stored in state and rendered for our users.

> 💡 The `fetch()` method is asynchronous, so we'll need to utilize `async/await` syntax when making requests.

## Service functions

When implementing the Fetch API within a React app, it's common practice to encapsulate fetching logic into dedicated modules known as *services*. These modules contain functions for making fetch requests, often grouped by the resource they are dealing with.

For example, we might have a `src/services/bookService.js` module for a `Book` resource. Inside `src/services/bookService.js`, we could include various service functions for creating, reading, updating, or deleting the `Book` resource.

When our React app needs to call upon a given service function, we can `import` it inside the appropriate component.

The benefits of this pattern include:

- **Modularity**: Service functions promote code reuse across an application by isolating API logic.
- **Separation of concerns**: Service functions keep the API logic separate from UI logic, making our components easier to read.
- **Testing and debugging**: Service functions make testing and debugging our code easier, making our apps more reliable.

While it is possible to write out fetching logic directly within a component, this will often make an application more difficult to maintain as it grows in complexity.

## Creating service functions

Let's create a dedicated service module for our requests to the Weather API.

Begin by creating a `services` directory inside `src`:

```bash
mkdir src/services
```

Then, create the service file:

```bash
touch src/services/weatherService.js
```

At the top of this file, we'll define a `BASE_URL`. This variable represents the initial part of the URL we wish to make requests to. It typically includes the protocol, domain name, and any path segments shared across the API's endpoints.

A `BASE_URL` simplifies making requests within different service functions that share the same starting point, as additional parameters or paths can be appended within each function.

Since each request to the Weather API requires our `API_KEY`, we'll incorporate this into our `BASE_URL` to avoid repetition in our service functions.

Add the following to `src/services/weatherService.js`:

```javascript
// src/services/weatherService.js

const API_KEY = '<YOUR_API_KEY_HERE>';
const BASE_URL = `http://api.weatherapi.com/v1/current.json?key=${API_KEY}`;
```

> 🚨 Remember to replace `<YOUR_API_KEY_HERE>` with your actual API Key!

Next, let's define an asynchronous function that will make our fetch request.

Our function will use the `fetch()` method to make a GET request to the endpoint as specified in our `BASE_URL`. When making this request, we'll need to include a query parameter specifying the city for which we want data.

Add the following to `src/services/weatherService.js`:

```javascript
// src/services/weatherService.js

const show = async (city) => {
  try {
    const queryString = `&q=${city}`;
    const res = await fetch(BASE_URL + queryString);
    const data = await res.json();
    console.log('Data:', data);
    return data;
  } catch (err) {
    console.log(err);
  }
};
```

In the code block above, we construct a request URL, send the request, and then parse the response as JSON. Both the `fetch()` and `json()` methods are asynchronous, so we include the `await` keyword to pause the execution of the code.

Notice how we define the `queryString` variable on its own line and then add it to the end of the `BASE_URL`. This makes our code a bit more legible.

> 💡 The [`.json()`](https://developer.mozilla.org/en-US/docs/Web/API/Response/json) method converts the JSON formatted string from the response into a JavaScript object.

Thanks to the addition of the Fetch API in Node 18, we can try out this function before implementing it in a React component.

At the bottom of the file, call upon `show()`, specifying the city of your choice:

```javascript
// src/services/weatherService.js

show('New York');
```

Run the following command in your terminal to execute the file:

```bash
node src/services/weatherService.js
```

If successful, you should see a response like the following in your terminal:

```plaintext
Data: {
  location: { ... },
  current: { ... }
}
```

## Implementing services in React components

With our service function built out, we can now use it within a React component.

First, let's be sure to `export` the function:

```javascript
// src/services/weatherService.js

// show('New York'); <=== Remove this line!

export { show };
```

Next, add the following `import` to the top of `src/App.jsx`:

```jsx
// src/App.jsx
import * as weatherService from './services/weatherService';
```

This syntax imports everything (`*`) exported from the `weatherService.js` module and groups them inside a `weatherService` object. Whenever we require a specific service function, we can access it through dot notation on the `weatherService` object.

Finally, add the following to the `App` component:

```jsx
// src/App.jsx
const App = () => {
  const fetchData = async () => {
    const data = await weatherService.show('New York');
    console.log('Data:', data);
  };

  return (
    <main>
      <h1>Weather API</h1>
      <button onClick={fetchData}>Fetch Weather Data</button>
    </main>
  );
};

export default App;
```

Here, we call upon `fetchData()` with an `onClick` event handler and log the data returned from `weatherService.show('New York')` to the console.

Try it out in your browser!
