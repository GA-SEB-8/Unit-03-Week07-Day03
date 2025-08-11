<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Update</span>
</h1>

**Learning Objective**: By the end of this lesson, students will be able to make update calls to an API and refresh the application's state to reflect those changes in a React application.

## Update a pet

Let's build update functionality! We'll start with a review of the requirements laid out in the intro:

- Conditionally render a form to update a pet.
- Handle the form submission.
- Display the updated pet in the UI.

And interpret them into a user story:

> 👤 As a user, I want to be able to update a pet entry via a form.

## Step 1: Conditionally render the form

We already have a form and a function that allows us to toggle it open and closed. We can use this same form to update a pet with one adjustment - when a user is updating a pet, we'll want to pass the currently selected pet object to the `PetForm` component to pre-fill the form inputs.

Inside `PetDetail.jsx`, let's create a new button that will call the function to toggle the form open and closed. As mentioned above, we'll want to pass the currently selected pet to the `handleFormView()` function:

```jsx
// src/components/PetDetail/PetDetail.jsx

    <div>
      <h1>{props.selected.name}</h1>
      <h2>Breed: {props.selected.breed}</h2>
      <h2>
        Age: {props.selected.age} year{props.selected.age > 1 ? 's' : ''} old
      </h2>
      {/* Our new button, wrapped in a div */}
      <div>
        <button onClick={() => props.handleFormView(props.selected)}>
          Edit Pet
        </button>
      </div>
    </div>
```

Make sure to pass `handleFormView` as a prop to the `PetDetail` component. While we're in `App.jsx`, let's also pass `selected` as a prop to `PetForm`, as we'll need this object to pre-fill our form if the user is editing a pet:

```jsx
// src/App.jsx

    <>
      <PetList
        pets={pets}
        handleSelect={handleSelect}
        handleFormView={handleFormView}
        isFormOpen={isFormOpen}
      />
      {/* Pass selected to PetForm and handleFormView to PetDetail */}
      {isFormOpen ? (
        <PetForm handleAddPet={handleAddPet} selected={selected} />
      ) : (
        <PetDetail selected={selected} handleFormView={handleFormView}/>
      )}
    </>
```

Before we move on, let's make one more adjustment to our code. Currently, our `handleFormView()` function doesn't accept any arguments:

```jsx
// src/App.jsx

  const handleFormView = () => {
    setIsFormOpen(!isFormOpen);
  };
```

Now that a user can either toggle the form view from `PetList` (to create a new pet) or toggle the form view from `PetDetail` (to edit an existing pet), we need a way to distinguish between the two potential uses of this function.

There are a few ways we could do this, but a simple method is to check whether the `handleFormView()` function has been passed a pet object. If not, we can presume the **New Pet** button has been pressed and set `selected` to null:

```jsx
// src/App.jsx

  const handleFormView = (pet) => {
    if (!pet._id) setSelected(null);
    setIsFormOpen(!isFormOpen);
  };
```

In `PetForm`, now that we have `selected` being passed as a prop, we can use it to determine the initial state of our form.

To make life easier, let's move the in-line initial state we currently have:

```jsx
// src/components/PetForm/PetForm.jsx

  const [formData, setFormData] = useState({
    name: '',
    age: '',
    breed: '',
  });
```

Into a variable called `initialState`:

```jsx
// src/components/PetForm/PetForm.jsx

  const initialState = {
    name: '',
    age: '',
    breed: ''
  }
  const [formData, setFormData] = useState(initialState)
```

Next, we can check if `selected` has a value.

If the user calls the `handleFormView()` from `PetDetail`, `selected` should be whatever pet the user chose.

Otherwise, the user will have clicked on the **New Pet** button in `PetList`, which sets `selected` to null. So, we can set the initial `formData` state based on whether `props.selected` has a truthy (non-null) value:

```jsx
// src/components/PetForm/PetForm.jsx

  const initialState = {
    name: "",
    age: "",
    breed: "",
  }
  // If pet data has been passed as props, we set formData as that pet object.
  // Otherwise, we can assume this is a new pet form and use the empty
  // initialState object.
  const [formData, setFormData] = useState(
    props.selected ? props.selected : initialState
  )
```

We can use the same ternary logic to provide some minor UI feedback to the user by conditionally rendering the submit button text:

```jsx
// src/components/PetForm/PetForm.jsx

      <form onSubmit={handleSubmit}>
        {/* Code for form inputs here */}
        <button type="submit">
          {props.selected ? 'Update Pet' : 'Add New Pet'}
        </button>
      </form>
```

Test it out in the browser and make sure the form is populated with the selected pet when the **Edit Pet** button is clicked.

## Step 2: Handle the form submission and display the updated pet

Next, we'll need to work with the back-end app to update a pet!

### 🎓 You Do: Complete the updating a pet functionality

Hopefully, this pattern is starting to become familiar - take some time to try to solve this independently using the following instructions. At the end of each step, you should have a new piece of functionality you can test.

1. Handle form submission.

   - In `App.jsx`, we'll want to:
     - Create a handler function called `handleUpdatePet()`, which will eventually invoke the service function and update the `pets` state.
     - This function should accept `formData` and a pet's `_id` as an argument.
     - Pass this new function as a prop to the `PetForm` component.
   - In the `PetForm` component, update the `handleSubmit()` function to:
     - Call the `handleUpdatePet()` function if there is a selected pet.
     - If there is no selected pet, call the create pet function.

       ```jsx
       // src/components/PetForm.jsx

          const handleSubmit = (evt) => {
            evt.preventDefault();
            if (props.selected) {
              // Don't forget to pass both formData and the ._id!
              props.handleUpdatePet(formData, props.selected._id);
            } else {
              props.handleAddPet(formData);
            }
          };
       ```

2. Get the form data to the service.

   - Inside of `petService.js`:
     - Create a new function called `update()`. This function should accept a pet object (`formData`) and a pet's `_id` as an argument.
     - Export the function.
   - In the `App` component's `handleUpdatePet()` function:
     - Call the new `update()` service and pass it the `formData` and pet's `_id`.
     - Assign the response to a variable called `updatedPet`.
     - Console log the `updatedPet` object (right now, it will log `undefined`).

3. Make the request to the back-end app and process the response.

   - Return to the `update()` function in `petService.js` and complete the function:
     - Use a `try...catch` block to handle any errors.
     - Use `fetch` to make a `PUT` request to the base URL, including the `_id` of the pet (check your server code if you need clarity on the exact URL).
     - Attach the `formData` object to the request body.
     - Use the `json()` method to parse the response.
     - Return the parsed response.
   - If you test this now, you should see the updated pet object logged to the console (because of the `console.log(updatedPet)` in `handleUpdatePet()` in the `App` component).

4. Update the `pets` state.

   - In the `App` component's `handleUpdatePet()` function:
     - Use the array's `map()` to update the `pets` state with the updated pet.
     - Close the form and set the `selected` state to the `updatedPet` object.
     - If `updatedPet` has an error, throw a new `Error` and log the error. Use a `try...catch` block to handle this.

Test it out in the browser and make sure you can update a pet and see it displayed in the UI.

### Check your work

If you run into any issues, you can check your work below:

There are a few ways to handle updating state with an edited pet. Below, we use `.map()` to test each element's `._id`, replacing the existing entry with the updated one only if the `_id`s match.

```jsx
// src/App.jsx

const handleUpdatePet = async (formData, petId) => {
  try {
    const updatedPet = await petService.update(formData, petId);

    // handle potential errors
    if (updatedPet.err) {
      throw new Error(updatedPet.err);
    }

    const updatedPetList = pets.map((pet) => (
      // If the _id of the current pet is not the same as the updated pet's _id,
      // return the existing pet.
      // If the _id's match, instead return the updated pet.
      pet._id !== updatedPet._id ? pet : updatedPet
    ));
    // Set pets state to this updated array
    setPets(updatedPetList);
    // If we don't set selected to the updated pet object, the details page will
    // reference outdated data until the page reloads.
    setSelected(updatedPet);
    setIsFormOpen(false);
  } catch (err) {
    console.log(err);
  }
};
```

Don't forget to pass `handleUpdatePet` as a prop to the `PetForm` component.

```jsx
// src/App.jsx

    <>
      <PetList
        pets={pets}
        handleSelect={handleSelect}
        handleFormView={handleFormView}
        isFormOpen={isFormOpen}
      />
      {/* Pass handleUpdatePet to PetForm */}
      {isFormOpen ? (
        <PetForm
          handleAddPet={handleAddPet}
          selected={selected}
          handleUpdatePet={handleUpdatePet}
        />
      ) : (
        <PetDetail selected={selected} handleFormView={handleFormView}/>
      )}
    </>
```

```js
// src/services/PetService.js

const update = async (formData, petId) => {
  try {
    const res = await fetch(`${BASE_URL}/${petId}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData),
    });
    return res.json();
  } catch (err) {
    console.log(err);
  }
};
```
