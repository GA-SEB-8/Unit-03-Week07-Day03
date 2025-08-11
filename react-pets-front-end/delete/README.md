<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Delete</span>
</h1>

**Learning Objective**: By the end of this lesson, students will be able to implement delete functionality in a React application, enabling them to make delete requests to an API and remove items from the user interface dynamically.

## Delete a pet

In this section, we'll build the delete functionality. Let's review the requirements laid out in the intro:

- Add a button to delete a pet.
- Remove the pet from the UI.

And translate them into a user story:

> 👤 As a user, I want to be able to click a button on the details view to delete a pet.

## 🎓 You Do: Implement delete functionality

Take some time to complete the following steps by following the instructions below and cross-referencing them with your existing code as needed.

1. Handle the delete button click event.

   - In the `App` component (where our `pets` state is), create a new function called `handleDeletePet()` to handle the deletion of a pet. This function will eventually invoke a service function and update the `pets` state.
     - This function should accept the `_id` of the pet as an argument.
     - Pass `handleDeletePet` as a prop to the `PetDetail` component.
   - In the `PetDetail` component:
     - Add a new button inside the same `div` as the **Edit Pet** button. The new button should have the text **Delete Pet**.
     - Make the button call the `handleDeletePet()` function when clicked, passing the `_id` of the selected pet.

2. Get the `_id` to the service.

   - Inside of `petService.js`:
     - Create a new function called `deletePet()`. This function should accept a pet's `_id` as an argument.
     - Export the function.
   - In the `App` component's `handleDeletePet()` function:
     - Call the new `deletePet()` service and pass it the pet's `_id`.
     - Assign the response to a variable called `deletedPet`.
     - Console log the `deletedPet` object (right now, it will log `undefined`).

3. Make the request to the back-end and process the response.

   - Return to the `deletePet()` function in `petService.js` and complete the function:
     - Use a `try...catch` block to handle any errors.
     - Use `fetch` to make a `DELETE` request to the base URL with the `_id` of the pet (check your server code if you need clarity on the exact URL).
     - Use the `json()` method to parse the response.
     - Return the parsed response.
   - If you test this now, you should see the delete pet object logged to the console (because of the `console.log(deletedPet)` in `handleDeletePet()` in the `App` component).

4. Update the `pets` state.

   - In the `App` component's `handleDeletePet()` function:
     - Use the array's `filter()` method to remove the deleted pet from `pets` state.
     - This function should set the `selected` state to `null` and the `isFormOpen` state to `false`.
     - If `deletedPet` has an error, throw a new `Error` and log the error. Use a `try...catch` block to handle this.

Test it out in the browser and make sure you can update a pet and see it displayed in the UI.

### Check your work

#### Step 1

```jsx
// src/components/PetDetail/PetDetail.jsx

const PetDetail = (props) => {
  if (!props.selected) {
    return (
      <div>
        <h1>NO DETAILS</h1>
      </div>
    );
  }

  return (
    <div>
      <h1>{props.selected.name}</h1>
      <h2>Breed: {props.selected.breed}</h2>
      <h2>
        Age: {props.selected.age} year{props.selected.age > 1 ? 's' : ''} old
      </h2>
      <div>
        <button onClick={() => props.handleFormView(props.selected)}>
          Edit Pet
        </button>
        <button onClick={() => props.handleDeletePet(props.selected._id)}>
          Delete Pet
        </button>
      </div>
    </div>
  );
};

export default PetDetail;
```

```js
// src/services/petService.js

const deletePet = async (petId) => {
  try {
    const res = await fetch(`${BASE_URL}/${petId}`, {
      method: 'DELETE',
    });
    return res.json();
  } catch (err) {
    console.log(err);
  }
};
```

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

import * as petService from './services/petService';

import PetList from './components/PetList/PetList';
import PetDetail from './components/PetDetail/PetDetail';
import PetForm from './components/PetForm/PetForm';

function App() {
  const [pets, setPets] = useState([]);
  const [selected, setSelected] = useState(null);
  const [isFormOpen, setIsFormOpen] = useState(false);

  useEffect(() => {
    const fetchPets = async () => {
      try {
        const fetchedPets = await petService.index();

        if (fetchedPets.err) {
          throw new Error(fetchedPets.err);
        }

        setPets(fetchedPets);
      } catch (err) {
        console.log(err);
      }
    };
    fetchPets();
  }, []);

  const handleSelect = (pet) => {
    setSelected(pet);
    setIsFormOpen(false);
  };

  const handleFormView = (pet) => {
    if (!pet._id) setSelected(null);
    setIsFormOpen(!isFormOpen);
  };

  const handleAddPet = async (formData) => {
    try {
      const newPet = await petService.create(formData);

      if (newPet.err) {
        throw new Error(newPet.err);
      }

      setPets([newPet, ...pets]);
      setIsFormOpen(false);
    } catch (err) {
      console.log(err);
    }
  };

  const handleUpdatePet = async (formData, petId) => {
    try {
      const updatedPet = await petService.update(formData, petId);

      if (updatedPet.err) {
        throw new Error(updatedPet.err);
      }

      const updatedPetList = pets.map((pet) => (
        pet._id !== updatedPet._id ? pet : updatedPet
      ));

      setPets(updatedPetList);
      setSelected(updatedPet);
      setIsFormOpen(false);
    } catch (err) {
      console.log(err);
    }
  };

  const handleDeletePet = async (petId) => {
    try {
      const deletedPet = await petService.deletePet(petId);

      if (deletedPet.err) {
        throw new Error(deletedPet.err);
      }

      setPets(pets.filter((pet) => pet._id !== deletedPet._id));
      setSelected(null);
      setIsFormOpen(false);
    } catch (err) {
      console.log(err);
    }
  };

  return (
    <>
      <PetList
        pets={pets}
        handleSelect={handleSelect}
        handleFormView={handleFormView}
        isFormOpen={isFormOpen}
      />
      {isFormOpen ? (
        <PetForm
          handleAddPet={handleAddPet}
          selected={selected}
          handleUpdatePet={handleUpdatePet}
        />
      ) : (
        <PetDetail
          selected={selected}
          handleFormView={handleFormView}
          handleDeletePet={handleDeletePet}
        />
      )}
    </>
  );
}

export default App;
```

## Congratulations! 🎉

You've successfully completed a full-stack project featuring a React front-end and an Express API back-end, which together are capable of performing complete CRUD operations on a resource while providing a UI to elevate the user experience. This is a significant achievement in your learning journey!

***Well done building your first MERN (MongoDB, Express, React, and Node.js) stack project!*** This milestone sets a solid foundation for your future projects and development skills. Keep up the great work!
