<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Add CSS</span>
</h1>

**Learning Objective**: By the end of this lesson, the learner will be able to style their React components using the provided CSS styles.

## Adding CSS

Great job completing your first MERN stack application! Let's take it further by polishing that front-end dashboard with some CSS.

We've created the following styles to give your application a dashboard layout. Remove the existing styles from `App.css` and add these in their place:

```css
#root {
  min-height: 100vh;
  width: 100%;
  display: grid;
  grid-template-columns: fit-content(375px) 1fr;
}

.sidebar-container {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  background-color: rgb(42, 42, 42);
  min-width: 0;
}

.list-container {
  display: flex;
  justify-content: flex-start;
  overflow-y: overlay;
  max-height: 700px;
  scrollbar-color: #ffffff40 #00000000;
  width: 100%;
}

ul {
  padding-left: 0;
  list-style-type: none;
  max-width: 300px;
  display: inline-block;
}

li {
  font-size: 2rem;
  white-space: wrap;
  text-overflow: ellipsis;
  overflow: hidden;
  margin-bottom: 1.2rem;
  cursor: pointer;
}

.details-container {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

label {
  font-size: 1.2rem;
}

input {
  height: 2rem;
  width: 12rem;
  margin-bottom: 1rem;
}

button {
  width: 200px;
  margin: 1rem;
}

.button-container {
  padding: 2rem;
  outline: 0;
}
```

## Add classes to components

We'll add some classes to our components to apply these styles. Before you begin, import `App.css` at the top of `App.jsx`.

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

// Importing the CSS file
import './App.css';

import * as petService from './services/petService';

import PetList from './components/PetList/PetList';
import PetDetail from './components/PetDetail/PetDetail';
import PetForm from './components/PetForm/PetForm';
```

### 1. `PetList` Component

Make these changes:

- **Class `sidebar-container`**: Add this class to the outermost `div` of the `PetList` component to style the sidebar where the pet list will appear.
- **Class `list-container`**: Add this class to the `div` that wraps the `ul` element containing the list of pets. This will style the container of the list, particularly its scrolling behavior.

<!-- {% raw %} -->

```jsx
// src/components/PetList/PetList.jsx

  return (
    <div className="sidebar-container">
      <h1>Pet List</h1>
      <div className="list-container">
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
      <button onClick={props.handleFormView}>
        {props.isFormOpen ? 'Close Form' : 'New Pet'}
      </button>
    </div>
  );
```

<!-- {% endraw %} -->

### 2. `PetDetail` Component

Make these changes:

- **Class `details-container`**: Add this class to the outermost `div` of the `PetDetail` component. This will apply styles to the details display sections, enhancing alignment and padding.
- **Class `button-container`**: Add this class to the `div` that wraps the edit and delete buttons to apply specific styling, like button padding.

```jsx
// src/components/PetDetail/PetDetail.jsx

if (!props.selected)
  return (
    <div className="details-container">
      <h1>NO DETAILS</h1>
    </div>
  );

return (
  <div className="details-container">
    <h1>{props.selected.name}</h1>
    <h2>Breed: {props.selected.breed}</h2>
    <h2>
      Age: {props.selected.age} year{props.selected.age > 1 ? 's' : ''} old
    </h2>

    <div className="button-container">
      <button onClick={() => props.handleFormView(props.selected)}>
        Edit Pet
      </button>
      <button onClick={() => props.handleDeletePet(props.selected._id)}>
        Delete Pet
      </button>
    </div>
  </div>
);
```

### 3. `PetForm` Component

Make these changes:

- **Class `form-container`**: Add this class to the outermost `div` that wraps the form element. This ensures that the form is centered and the flexbox properties are applied correctly.
  - Additionally, it ensures all inputs and labels are properly aligned and spaced by the inherited CSS from the `form` tag.

```jsx
// src/components/PetForm/PetForm.jsx

  return (
    <div className="form-container">
      <form onSubmit={handleSubmit}>
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
        <button type="submit">
          {props.selected ? 'Update Pet' : 'Add New Pet'}
        </button>
      </form>
    </div>
  );
```

And you're done - that's a great looking dashboard!
