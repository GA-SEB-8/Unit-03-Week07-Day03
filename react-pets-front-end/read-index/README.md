<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Read Index</span>
</h1>

**Learning Objective**: By the end of this lesson, the learner will be able to connect a React application to an API to fetch and display a list of items, specifically showing all pets on the screen.

## Read all pets

We'll start by completing the index read actions. As a reminder, the requirement laid out in the intro was to display all pets in the UI.

We can translate this into a user story:

> 👤 As a user, I want to be able to view all of the pets.

## Creating the service

First, let's create the service function to make a [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) call to the API. This pattern will become more familiar with practice, so we'll take some time to detail each step for this first function.

Inside of `petService.js`, create a new async function called `index`:

```javascript
// src/services/petService.js

const index = async () => {};
```

Why async? The service functions we'll build for this lesson will make asynchronous fetch calls to the API. We'll need to use an `async` function to `await` the response from our fetch calls.

Next, let's go ahead and code the fetch call. We've already created a `BASE_URL` variable and assigned it to the base URL of our API (in this case, `http://localhost:3000/pets`). Since we're making a GET request (the default method for `fetch()`) to `/pets`, we need to pass `BASE_URL` to the `fetch()` function:

```javascript
// src/services/petService.js

const index = async () => {
  const res = await fetch(BASE_URL);
};
```

The `index()` function will get back a `Response` object and assign it to a new `res` variable.

To resolve this response to a usable JavaScript object, we need to invoke its [json()](https://developer.mozilla.org/en-US/docs/Web/API/Response/json) method. We also know that, on the front end, we'll want to use this data to populate a list of pets, so we'd better return the parsed `res` data out of this function:

```javascript
// src/services/petService.js

const index = async () => {
  const res = await fetch(BASE_URL);
  return res.json();
};
```

Our final step is to handle potential errors - let's wrap our fetch call in a `try...catch` block and log any errors to the console:

```javascript
// src/services/petService.js

const index = async () => {
  try {
    const res = await fetch(BASE_URL);
    return res.json();
  } catch (err) {
    console.log(err);
  }
};
```

In summary, this function:

- Uses `fetch()` to make a `GET` request to the base URL.
- Parses the response using the `json()` method.
- Handles errors with a `try...catch` block.
- Returns the parsed response.

Make sure to export this function! At this stage, your `petService.js` file should look like this:

```javascript
// src/services/petService.js

const BASE_URL = `${import.meta.env.VITE_BACK_END_SERVER_URL}/pets`;

const index = async () => {
  try {
    const res = await fetch(BASE_URL);
    return res.json();
  } catch (err) {
    console.log(err);
  }
};

export {
  index,
};
```

> 🧪 You can test the `index()` function before you do anything else, so troubleshooting is easier later! Add this line between the `index()` function's definition and the export:
>
> ```javascript
> console.log(await index());
> ```
>
> If everything is working correctly, you should see an array of pets in the console, and you can remove that line before moving on. If you see an error, the time to troubleshoot is now! Check that your server is running and your database is populated with at least one pet!
>
> Use this pattern as you build out service functions that retrieve data - it's a great way to ensure each function works as expected before moving on to the next step.

## Setting state with the service

Next, let's work on the UI to make the service call and render the data it returns. To do this, we'll use two hooks - `useState()` and `useEffect()`. Let's go ahead and import those in `App.jsx` now:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';
```

Inside `App.jsx`, create a new state variable called `pets` to hold the pets. We know that the pet data we'll get back from our API should be an array, so we'll give `pets` the initial value of an empty array:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

const App = () => {
  const [pets, setPets] = useState([]);

  return <h1>Hello world!</h1>;
};

export default App;
```

Next, let's create a `useEffect()` hook to call the `index` service function.

First, we'll need to import our `index` function. For now, we only have a single function exported from `petService`, but eventually, multiple functions will come from the same file. To save ourselves from having to refactor later, let's go ahead and import all (`*`) of the exported functions as methods on a new `petService` object:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

// Add an import for the petService functions
import * as petService from './services/petService';
```

All our pet service functions will live in one tidy object in `App.js`, regardless of how many we export.

Next up, let's work on the `useEffect()`. Let's talk about what it will do first.

When the page loads, we want to make a fetch call to our API, courtesy of our index service.

The `useEffect()` hook works great for that purpose, but there is one notable downside. Our `index()` function is asynchronous, meaning we'll need to `await` any data it may return. Unfortunately, the callback function we pass to `useEffect()` cannot be made to be `async`:

```jsx
// This will not work

useEffect(async () => {
  await something;
}, []);
```

How can we get around this limitation? Other than abandoning `async/await` syntax and returning to the potential callback hell of `.then()` syntax, a common workaround is to create a new `async` function inside of the callback, and then invoke it immediately afterward:

```jsx
// This WILL work

useEffect(() => {
  const newFunction = async () => {
    const data = await service();
  };
  newFunction();
}, []);
```

> 🧠 When making a function that is invoked immediately after creation, using an [Immediately Invoked Function Expression](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) (or IIFE) is also an option.

With that in mind, we can build out our `useEffect()`. Once we have the data back from our index function, we'll use it to set our `pets` state:

```jsx
// src/App.jsx

const App = () => {
  const [pets, setPets] = useState([]);

  // Create a new useEffect
  useEffect(() => {
    // Create a new async function
    const fetchPets = async () => {
      // Call on the pet service's index function
      const fetchedPets = await petService.index();
      // Set pets state to the returned pets data
      setPets(fetchedPets);
    };
    // Invoke the function
    fetchPets();
    // Add an empty dependency array to the `useEffect()` hook.
  }, []);

  return <h1>Hello world!</h1>;
};
```

### Error handling

Let's take a second to talk about error handling for our application. In the back-end, the following Express controller function is responding to our request to `/pets`:

*Express server code:*

```javascript
// READ - GET - /pets
router.get('/', async (req, res) => {
  try {
    const foundPets = await Pet.find();
    res.status(200).json(foundPets);
  } catch (err) {
    res.status(500).json({ err: err.message });
  }
});
```

From this, we know that our response will be one of two things:

- An array of pets
- An object with an error property

In the instance that the API sends back an error object, we know something went wrong! Using an `if` statement, we can check if the response has an `err` property, which will only be the case if our controller caught an error. If so, we'll throw a new `Error` - this time in our handler function - and pass the error contents to the `catch` block.

Throwing an error also prevents statements after the `throw` from being executed, so we don't have to worry about accidentally setting `pets` state to an error object.

```jsx
// src/App.jsx

  useEffect(() => {
    const fetchPets = async () => {
      try {
        const fetchedPets = await petService.index();
        // Don't forget to pass the error object to the new Error
        if (fetchedPets.err) {
          throw new Error(fetchedPets.err);
        }
        setPets(fetchedPets);
      } catch (err) {
        // Log the error object
        console.log(err);
      }
    };
    fetchPets();
  }, []);
```

In the `fetchPets`'s `catch` block, we log the error. For this app, having any server-side issues show up in the client-side console is enough to help make debugging easier. This type of error-handling setup also allows us to provide error feedback in the UI; however, that's not within the scope of this particular lesson.

Check the state of your application in the React Dev tools to confirm your list of pets exists.

## Displaying the data

Next, let's set up our UI to display the pets in our state.

Inside of `src/components`, create a new `PetList` directory with a file called `PetList.jsx`:

```bash
mkdir src/components/PetList
touch src/components/PetList/PetList.jsx
```

In `PetList.jsx`, create a new function component called `PetList`. Make sure it accepts `props` as an argument. For now, it will log `props` and return an `h1` element wrapped in a `div`:

```jsx
// src/components/PetList/PetList.jsx

const PetList = (props) => {
  // Let's ensure we have data to work with before adding functionality!
  console.log(props);

  return (
    <div>
      <h1>Pet List</h1>
      <div></div>
    </div>
  );
};

export default PetList;
```

We've stubbed up the component and can pass data to it. Let's import the `PetList` component into `App.jsx`. Then, inside the return statement of `App.jsx`, render it!

Pass the `pets` state variable as a prop to the `PetList` component. This component is going to grow in complexity as we add more features, so we're preemptively wrapping the `PetList` component in a React fragment:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

import * as petService from './services/petService';

// Our new PetList import!
import PetList from './components/PetList/PetList';

const App = () => {
  const [pets, setPets] = useState([]);

  // useEffect() code here

  // Return the new PetList component inside a React fragment.
  return (
    <>
      <PetList pets={pets} />
    </>
  );
};

```

You should see the **Pet List** header in the browser and the `props` object in the `PetList` component logged to the console (as well as in your React DevTools). If you see objects in the `pets` property on `props`, you know that the `PetList` component is receiving the `pets` state!

Once you've confirmed the `PetList` component has the right data, you can remove the `console.log` statement.

### Rendering the list

Inside the return statement of `PetList.jsx`, create a `ul` element inside the existing empty `div` element. Render the list of pets inside the `ul`.

Each `<li>` element will display the `name` property of each pet in `pets`. React will also insist that each top-level element in a list must have a unique key - fortunately, MongoDB generates an ID (as `_id`) for us when creating a resource, so we can use `pet._id` for the key:

```jsx
// src/components/PetList/PetList.jsx

  return (
    <div>
      <h1>Pet List</h1>
      <div>
        <ul>
          {props.pets.map((pet) => (
            <li key={pet._id}>{pet.name}</li>
          ))}
        </ul>
      </div>
    </div>
  );
```

Test it out in the browser and make sure you see the list of pets.

## No pets?

Finally, let's handle the case where there are no pets to display.

This can be handled with a ternary operator, checking to see if the pets array has a length of 0. If so, we display a message that there are no pets to display, and if not, we instead display the list of pets:

```jsx
// src/components/PetList/PetList.jsx

  return (
    <div>
      <h1>Pet List</h1>
      <div>
        {!props.pets.length ? (
          <h2>No Pets Yet!</h2>
        ) : (
          <ul>
            {props.pets.map((pet) => (
              <li key={pet._id}>{pet.name}</li>
            ))}
          </ul>
        )}
      </div>
    </div>
  );
```
