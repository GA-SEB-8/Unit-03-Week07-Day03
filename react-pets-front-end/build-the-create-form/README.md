<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Build the Create Form</span>
</h1>

**Learning Objective**: By the end of this lesson, students will be able to create and manage forms within a React application to add new pet entries to a database.

## Create a new pet, part 1

Let's start on the create functionality for adding a new pet to our database. Once again, let's review the minimum requirements laid out in the intro:

- Conditionally render a form to create a new pet.
- Handle the form submission.
- Display the new pet in the UI.

And convert them into a user story:

> 👤 As a user, I want to be able to enter information into a form and use it to create a new pet entry.

We'll tackle create functionality in two steps. The first is to build the create form and conditionally render it. The second is to handle the form submission and display the new pet in the UI.

## Step 1: Build the create form

In this step, we'll set up a form where users can input new data for a pet. Typically, forms for creating new entries, like adding a new pet, are placed on separate pages. However, since we're not using multiple pages or routing in this lesson, we'll manage the form's visibility with state. This means we'll use state to control whether the form is displayed or hidden based on user interactions.

### Creating the form component

Inside the `components` directory, let's create a new directory called `PetForm` containing a file called `PetForm.jsx`.

```bash
mkdir src/components/PetForm
touch src/components/PetForm/PetForm.jsx
```

In `PetForm.jsx`, create a function component that accepts `props` as an argument. We'll return to this file and build out the form in a moment.

Make sure to export the `PetForm` component at the end of the file.

### Add the `PetForm` component to the `App` component

Start by importing the `PetForm` component, then render it on the page:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

import * as petService from './services/petService';

import PetList from './components/PetList/PetList';
import PetDetail from './components/PetDetail/PetDetail';
// Import the new PetForm component
import PetForm from './components/PetForm/PetForm';

const App = () => {
  const [pets, setPets] = useState([]);
  const [selected, setSelected] = useState(null);

  // useEffect and handleSelect code here

  return (
    <>
      <PetList pets={pets} handleSelect={handleSelect} />
      <PetForm />
      <PetDetail selected={selected} />
    </>
  );
};
```

### Create the form

Now that we can see what we're doing as we work, we'll build out the `PetForm` component. We'll start by creating a controlled form allowing users to input data (name, age, and breed) for a new pet.

We'll use the `useState()` hook to manage the form data and the `handleChange()` function to update the `formData` state as the user types.

```jsx
// src/components/PetForm/PetForm.jsx

import { useState } from 'react';

const PetForm = (props) => {
  // formData state to control the form.
  const [formData, setFormData] = useState({
    name: '',
    age: '',
    breed: '',
  });

  // handleChange function to update formData state.
  const handleChange = (evt) => {
    setFormData({ ...formData, [evt.target.name]: evt.target.value });
  };

  // And finally, the form itself.
  return (
    <div>
      <form>
        <label htmlFor="name"> Name </label>
        <input
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          required
        />
        <label htmlFor="age"> Age </label>
        <input
          id="age"
          name="age"
          value={formData.age}
          onChange={handleChange}
          required
        />
        <label htmlFor="breed"> Breed </label>
        <input
          id="breed"
          name="breed"
          value={formData.breed}
          onChange={handleChange}
        />
        <button type="submit">Add New Pet</button>
      </form>
    </div>
  );
};

export default PetForm;
```

### Conditionally render the form

When the page loads initially, the form should not be open. To accomplish this, we'll need to create a new state variable inside `App.jsx` to represent the open or closed form component. We can set the initial value to `false` to represent it being closed when we first load the `App` component:

```jsx
// src/App.jsx

  const [pets, setPets] = useState([]);
  const [selected, setSelected] = useState(null);
  // New state variable to control form visibility.
  const [isFormOpen, setIsFormOpen] = useState(false);
```

Next, still in `App.jsx`, create a `handleFormView()` function to toggle the above state variable. When the function is called, the Boolean value of `isFormOpen` should change from `false` to `true` or vice versa.

We also want to hide the form if it's open when the user clicks a pet to view its details. We'll add a call to `setIsFormOpen(false)` inside the existing `handleSelect()` function.

```jsx
// src/App.jsx

  const [pets, setPets] = useState([]);
  const [selected, setSelected] = useState(null);
  const [isFormOpen, setIsFormOpen] = useState(false);

  // useEffect code here.

  const handleSelect = (pet) => {
    setSelected(pet);
    // Close the form if it's open when a new pet is selected.
    setIsFormOpen(false);
  };

  const handleFormView = () => {
    setIsFormOpen(!isFormOpen);
  };
```

We'll control whether the form is displayed or hidden when the user clicks a button that will exist inside the `PetList` component. After creating the function, let's pass it down to `PetList` as a prop.

```jsx
// src/App.jsx
  return (
    <>
      <PetList
        pets={pets}
        handleSelect={handleSelect}
        handleFormView={handleFormView}
      />
      <PetForm />
      <PetDetail selected={selected} />
    </>
  );
```

The text of the button will need to change depending on whether the form is open. To determine what text the button should display to the user, we'll need to pass `isFormOpen` down to `PetList`.

```jsx
// src/App.jsx

  return (
    <>
      <PetList
        pets={pets}
        handleSelect={handleSelect}
        handleFormView={handleFormView}
        isFormOpen={isFormOpen}
      />
      <PetForm />
      <PetDetail selected={selected} />
    </>
  );
```

Finally, in `PetList.jsx`, we can create our button. It will call `handleFormView` when clicked, and we'll also change what text is displayed based on the value of `isFormOpen`.

- The button should say **Close Form** if the form is open (`true`).
- The button should say **Add Pet** if the form is closed (`false`).

<!-- {% raw %} -->

```jsx
// src/components/PetList.jsx

  return (
    <div>
      <h1>Pet List</h1>
      <div>
        {!props.pets.length ? (
          <h2>No Pets Yet!</h2>
        ) : (
          <ul>
            {props.pets.map((pet) => 
              <li 
                key={pet._id}
                style={{ cursor: 'pointer', color: "#646CFF" }}
                onClick={() => props.handleSelect(pet)}
              >
                {pet.name}
              </li>
            )}
          </ul>
        )}
      </div>
      {/* Our new button! */}
      <button onClick={props.handleFormView}>
        {props.isFormOpen ? 'Close Form' : 'New Pet'}
      </button>
    </div>
  );
```

<!-- {% endraw %} -->

To complete this step, we'll need to conditionally render either the `PetForm` component or the `PetDetail` based on the value of `isFormOpen`:

```jsx
// src/App.jsx

  return (
    <>
      <PetList
        pets={pets}
        handleSelect={handleSelect}
        handleFormView={handleFormView}
        isFormOpen={isFormOpen}
      />
      {isFormOpen ? (
        <PetForm />
      ) : (
        <PetDetail selected={selected} />
      )}
    </>
  );
```

Test it out in the browser and make sure you can toggle the form open and closed.
