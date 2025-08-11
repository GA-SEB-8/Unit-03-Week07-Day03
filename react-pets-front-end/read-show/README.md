<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Read Show</span>
</h1>

**Learning Objective**: By the end of this lesson, students will be able to use React components to display detailed information about an individual item from an API, specifically showing detailed views of a pet when selected.

## Read a single pet

As with the index section, let's review the minimum requirements for showing a single pet:

- Add a link to view the details of a single pet.
- Conditionally render the details of a single pet.

And the accompanying user story:

> 👤 As a user, I want to click a button to view the details of a single pet.

## Adding the UI for showing a single pet

Let's implement the ability to show a single pet in the UI. We want to be able to click on a pet and see its details.

<!-- {% raw %} -->

1. Inside `PetList.jsx`, where you `.map()` over the pets, we want to indicate to the user that a pet can be clicked on. We'll give it some inline styling to help accomplish this.

2. Next, add an `onClick` event to the `li` element. For testing purposes, let's add an inline function that logs the clicked pet to the console.

   ```jsx
   // src/components/PetList/PetList.jsx

           <ul>
             {props.pets.map((pet) => (
               <li 
                 key={pet._id}
                 style={{ cursor: 'pointer', color: "#646CFF" }}
                 onClick={() => console.log(pet)}
               >
                 {pet.name}
               </li>
             ))}
           </ul>
   ```

<!-- {% endraw %} -->

You should be able to click on each pet name and see the corresponding pet object log to the console. If not, debug before moving on to the next step!

## Handling the click event

When designing a webpage with routes, we would normally create a show page that could be linked to at `/pets/:petId`. This would keep track of which specific pet the user had selected.

This app does not use routing, so we'll need a way to track which pet the user has selected. That sounds like a job for state!

Create a new state variable that will hold a single pet. This should be an object that represents the selected pet or `null` if no pet is selected:

```jsx
// src/App.jsx

  const [selected, setSelected] = useState(null);
```

Next, we'll need to create a new function to handle the click event we set up in the `PetList` component. We'll build it inside `App.jsx`, as it will need access to the `setSelected` method we just set up.

This function should accept a pet object as an argument and set the selected pet to the state variable.

```jsx
// src/App.jsx

  const [pets, setPets] = useState([])
  const [selected, setSelected] = useState(null)

  // useEffect code here

  // Add the following:
  const handleSelect = (pet) => {
    setSelected(pet)
  }

  // Return statement here

```

Next, pass this function as a prop to the `PetList` component and call it when a pet is clicked. This should replace the console log we added earlier.

```jsx
// src/App.jsx

  return (
    <>
      <PetList pets={pets} handleSelect={handleSelect}/>
    </>
  );
```

<!-- {% raw %} -->

```jsx
// src/components/PetList/PetList.jsx

        <ul>
          {props.pets.map((pet) => (
            <li 
              key={pet._id}
              style={{ cursor: 'pointer', color: "#646CFF" }}
              // Call the handleSelect() function on click, passing the pet.
              onClick={() => props.handleSelect(pet)}
            >
              {pet.name}
            </li>
          ))}
        </ul>
```

<!-- {% endraw %} -->

Before moving on, test that you can click on a pet and have the `selected` state update to the selected pet. You can check this with the React DevTools.

## Pass the selected state to a new component

Now that we have the selected pet in state, we can conditionally render the pet's details in the UI.

Inside the `components` directory, create a new directory, `PetDetail`, containing a file called `PetDetail.jsx`.

```bash
mkdir src/components/PetDetail
touch src/components/PetDetail/PetDetail.jsx
```

In this file, create a function component called `PetDetail`, which accepts `props` as an argument.

Back in `App.jsx`, import the `PetDetail` component and add it to the return statement. Make sure to pass it the `selected` state containing our pet object on `props`.

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

import * as petService from './services/petService';

import PetList from './components/PetList/PetList';
// Import the new PetDetail component.
import PetDetail from './components/PetDetail/PetDetail';

const App = () => {
  const [pets, setPets] = useState([]);
  const [selected, setSelected] = useState(null);

  // useEffect code here

  const handleSelect = (pet) => {
    setSelected(pet);
  };

  return (
    <>
      <PetList pets={pets} handleSelect={handleSelect} />
      <PetDetail selected={selected} />
    </>
  );
};
```

You should now have `props.selected` available in the `PetDetail` component!

## 🎓 You Do: Add pet details markup

Inside the new `PetDetail` component, render the details of the selected pet, including the name, breed, and age information.

Use the patterns we've demonstrated so far to test as you go!

### Conditional rendering of selected pet

We will also want to conditionally render a header that reads 'NO DETAILS' instead of pet details if no pet is currently selected.

You can choose to do this in a couple of ways:

- In a single return statement, use a ternary operator to render the header or pet details based on the status of `props.selected`. Wrap all the elements returned in a div.

or

- If `props.selected` is falsy (`null`), return the header wrapped in a `div`. If `props.selected` is not falsy, proceed to the normal return with the pet's details, again wrapped in a `div`.

Either is fine - we'll demonstrate the second option, but both work well for this application.

Here's the solution:

```jsx
// src/components/PetDetail/PetDetail.jsx

const PetDetail = (props) => {
  // return if props.selected is null
  if (!props.selected) {
    return (
      <div>
        <h1>NO DETAILS</h1>
      </div>
    );
  }

  // return statement if props.selected has a truthy value
  return (
    <div>
      <h1>{props.selected.name}</h1>
      <h2>Breed: {props.selected.breed}</h2>
      <h2>
        Age: {props.selected.age} year{props.selected.age > 1 ? 's' : ''} old
      </h2>
    </div>
  );
};

export default PetDetail;
```

Make sure to export the `PetDetail` component at the end of the file! Test it out in the browser, and make sure you can click on a pet to see its details!
