<h1>
  <span class="headline">Fetching Data in React</span>
  <span class="subhead">Fetching Data with User Input</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to make a `fetch` request to the weather API using dynamic data from user input.

## Overview

When we fetch data based on user actions, like submitting a form or clicking a link, we often need to include additional information within our fetch request.

With a link, this additional information might be a unique identifier for a given resource. With a search bar, we would need to include the search term.

In this lesson, we focus on building a feature allowing users to search for weather information by entering a city name in a search bar. We'll build out a search component to take in user input, fetch the weather data, store it in state, and then display it to the user in a dedicated component.

## Building the search form component

Let's start by building the search component. With this component, a user should be able to enter the name of a city and submit the form.

Run the following commands in your terminal:

```bash
mkdir src/components src/components/WeatherSearch
touch src/components/WeatherSearch/WeatherSearch.jsx
```

Add the following to `src/components/WeatherSearch/WeatherSearch.jsx`:

```jsx
// src/components/WeatherSearch/WeatherSearch.jsx

import { useState } from 'react';

const WeatherSearch = (props) => {
  const [city, setCity] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    // We'll call the fetch function here
    setCity('');
  };

  return (
    <section>
      <h2>Search</h2>
      <form onSubmit={handleSubmit}>
        <label htmlFor="city">Enter a city:</label>
        <input
          id="city"
          type="text"
          value={city}
          onChange={(e) => setCity(e.target.value)}
        />
        <button type="submit">Search</button>
      </form>
    </section>
  );
};

export default WeatherSearch;
```

Next, we'll need to display the `WeatherSearch` component.

Add the following import at the top of `src/App.jsx`:

```jsx
// src/App.jsx
import WeatherSearch from './components/WeatherSearch/WeatherSearch';
```

Update the `return` in `src/App.jsx` with the following:

```jsx
// src/App.jsx

return (
  <main>
    <h1>Weather API</h1>
    <WeatherSearch />
  </main>
);
```

In your browser, you should now be able to type information into the search field and see it added to state.

## Submitting a search

With our search form in place, we can now build out the remaining functionality. When a user submits a request for weather information, we'll want to store the response data in `weather` state.

Add the following import to the top of `src/App.jsx`:

```jsx
// src/App.jsx
import { useState } from 'react';
```

And define the following `useState`:

```jsx
// src/App.jsx
const [weather, setWeather] = useState({});
```

Next, we'll need to make a few changes to our `fetchData` function.

Our `fetchData` function will need to accept a `city` string as an argument and `setWeather` state based on the `data` we get as a response.

The information we get back from the Weather API is quite detailed, but the data structure used in these responses is not ideal for rendering. In a situation like this, it's often a good idea to clean up the shape of the data before setting it in state.

There are three pieces of information we want to display for our users:

1. The name of the location.
2. The current temperature of the location.
3. The current weather conditions for that location.

Given that, we can create a new object like the one below and set that to state:

```jsx
const newWeatherState = {
  location: data.location.name,
  temperature: data.current.temp_f,
  condition: data.current.condition.text,
};
```

Refactor your `fetchData` function as shown below:

```jsx
// src/App.jsx

const fetchData = async (city) => {
  const data = await weatherService.show(city);
  const newWeatherState = {
    location: data.location.name,
    temperature: data.current.temp_f,
    condition: data.current.condition.text,
  };
  setWeather(newWeatherState);
};
// The following log should be outside of the fetchData function
console.log('State:', weather);
```

> 💡 Why aren't we logging `weather` within `fetchData`?
>
> This is due to the asynchronous nature of state updates. If we tried to log state within the function, we would see an out-of-date record of state.
>
> By logging it outside the function but within the component, we can see the current value held in state when the component re-renders.

To lift state back to `App`, we'll need to pass our `fetchData` function down into our `WeatherSearch` component.

Pass `fetchData` down to `<WeatherSearch />` as a prop:

```jsx
// src/App.jsx


return (
  <main>
    <h1>Weather API</h1>
    <WeatherSearch fetchData={fetchData} />
  </main>
);
```

And update `handleSubmit` with the following:

```jsx
// src/components/WeatherSearch/WeatherSearch.jsx

const handleSubmit = (e) => {
  e.preventDefault();
  props.fetchData(city); // Don't forget to pass city state!
  setCity('');
};
```

Check your browser console or React Dev Tools. You should see weather information is now held in the `weather` state!

## Displaying weather details

With state being updated correctly, it's time to render this data for our users. We'll accomplish this in a dedicated component called `WeatherDetails`.

Run the following command in your terminal:

```bash
mkdir src/components/WeatherDetails
touch src/components/WeatherDetails/WeatherDetails.jsx
```

Let's build out a basic HTML structure for the component.

Add the following to `src/components/WeatherDetails/WeatherDetails.jsx`:

```jsx
// src/components/WeatherDetails/WeatherDetails.jsx

const WeatherDetails = (props) => {
  console.log('WeatherDetails props:', props);
  return (
    <section>
      <h2>Weather Details</h2>
      <p>Location:</p>
      <p>Temperature:</p>
      <p>Condition:</p>
    </section>
  );
};

export default WeatherDetails;
```

Add the following import to `src/App.jsx`:

```jsx
// src/App.jsx
import WeatherDetails from './components/WeatherDetails/WeatherDetails';
```

Next, add `<WeatherDetails />` to the `return` with `weather` included as a prop:

```jsx
// src/App.jsx

return (
  <main>
    <h1>Weather API</h1>
    <WeatherSearch />
    <WeatherDetails weather={weather} />
  </main>
);
```

Confirm that `weather` has been passed down through props.

Finally, update the `return` in `src/components/WeatherDetails/WeatherDetails.jsx` with the following:

```jsx
// src/components/WeatherDetails/WeatherDetails.jsx
return (
  <section>
    <h2>Weather Details</h2>
    <p>Location: {props.weather.location}</p>
    <p>Temperature: {props.weather.temperature}</p>
    <p>Condition: {props.weather.condition}</p>
  </section>
);
```

You should now be able to view the result of a search. Try it out in your browser!
