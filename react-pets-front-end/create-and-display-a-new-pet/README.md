<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Create and Display a New Pet</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create a new pet and display it in the UI.

## Create a new pet, part 2

As a reminder of where we are in the process, let's review the minimum requirements for creating a new pet:

- Conditionally render a form to create a new pet. This is done! 🎉
- Handle the form submission.
- Display the new pet in the UI.

And here's our corresponding user story:

> 👤 As a user, I want to be able to enter information into a form and use it to create a new pet entry.

Let's wrap up our create functionality.

## Step 2: Handle the form submission and display the new pet

Because creating a new pet will impact our `pets` state, we'll want to place the new function where that state lives - `App.jsx`.

In `App.jsx`, create a new async function named `handleAddPet()`, which accepts an object (our form data) as an argument. Add the structure for a `try...catch` statement as well.

Eventually, this function will call a new `petService` function - `create()` - and pass it the form data. `create()` will return the created pet object from the database, which we can add to the `pets` state.

```jsx
// src/App.jsx

const App = () => {

  // State variables, useEffect, handleSelect, and handleFormView code here.

  const handleAddPet = async (formData) => {
    try {
  
    } catch (err) {
      console.log(err);
    }
  };

  // return code here.
};
```

Pass the new `handleAddPet()` function as a prop to the `PetForm` component.

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
        <PetForm handleAddPet={handleAddPet} />
      ) : (
        <PetDetail selected={selected} />
      )}
    </>
  );
```

### Handle form submission

Inside of `PetForm.jsx` we can now create a new function called `handleSubmit()` to call when the the form is submitted.

This function will accept the submit event as an argument. To prevent the page from reloading, we'll call `evt.preventDefault()`. Then, we'll call `props.handleAddPet()` and pass it `formData`.

```jsx
// src/components/PetForm/PetForm.jsx

const PetForm = (props) => {

  // State variables and handleChange code here.

  const handleSubmit = (evt) => {
    evt.preventDefault();
    props.handleAddPet(formData);
    // Right now, if you add a pet and submit the form,
    // the data entered will stay on the page. We'll fix this soon.
  };

  return (
    <div>
      {/* Call the new handleSubmit function when the form is submitted. */}
      <form onSubmit={handleSubmit}>
        {/* Form elements here. */}
      </form>
    </div>
  );
};    
```

> 🧪 You can test that this works by adding a `console.log(formData)` inside the `handleAddPet()` function in the `App` component and then submit a completed form.

### Create the service

Inside `petService.js`, we'll need to add a new function called `create()`.

Stub up the function so that it:

- Accepts a pet object as an argument.
- Will be able to handle errors with a `try...catch` block.

We'll come back to this soon to add the actual functionality.

```jsx
// src/services/petService.js

const create = async (formData) => {
  try {

  } catch (err) {
    console.log(err);
  }
};
```

Make sure to export the `create()` function at the bottom of the file.

### Call the service

With our `create()` function stubbed up, we can pass it data from the `handleAddPet()` function in our `App` component.

```jsx
// src/App.jsx

  const handleAddPet = async (formData) => {
    try {
      // Call petService.create, assign return value to newPet
      const newPet = await petService.create(formData);
      // Log the newPet to the console
      console.log(newPet);
    } catch (err) {
      console.log(err);
    }
  };
```

Now that the `create()` function receives data, we can build the functionality to send that data to the back-end.

### Send the data to the back-end and handle the response

Time to create a new pet in the database! We'll need to:

- Use `fetch()` to make a `POST` request to the base URL and send the new pet data with the request.
- Parse the response with the `json()` method.
- Return the parsed response.

This will resemble the service call we made while building index functionality, except we'll make a `'POST'` request now instead of a `'GET'` request. This means we'll need to specify a few things in the `options` object:

```jsx
// src/services/petService.js

const create = async (formData) => {
  try {
    const res = await fetch(BASE_URL, {
      // We specify that this is a 'POST' request
      method: 'POST',
      // We're sending JSON data, so we attach a Content-Type header
      // and specify that the data being sent is type 'application/json'
      headers: {
        'Content-Type': 'application/json',
      },
      // The formData, converted to JSON, is sent as the body
      // This will be received on the back-end as req.body
      body: JSON.stringify(formData),
    });
    return res.json();
  } catch (err) {
    console.log(err);
  }
};
```

> 🧪 Test this out in the browser. You should see the new pet object logged to the console when you submit the form.

### Add the new pet to state

We'll invoke `setPets()` and pass it a new array comprised of the new pet object, followed by the existing `pets`, which we'll add using the spread operator.

```jsx
// src/App.jsx

  const handleAddPet = async (formData) => {
    try {
      const newPet = await petService.create(formData);
      // Add the pet object and the current pets to a new array, and
      // set that array as the new pets
      setPets([newPet, ...pets]);
    } catch (err) {
      console.log(err);
    }
  };
```

Once the user has submitted the form, we want the form to close - this, along with the new pet showing up in the `PetList` component, will serve as feedback to the user that the form was submitted.

Fortunately, all we need to do to accomplish that is call `setIsFormOpen()` and pass it `false` at the end of the `handleAddPet()` function.

```jsx
// src/App.jsx

  const handleAddPet = async (formData) => {
    try {
    const newPet = await petService.create(formData);

    setPets([newPet, ...pets]);
    // Close the form after we've added the new pet
    setIsFormOpen(false);
    } catch (err) {
      console.log(err);
    }
  };
```

### Error handling

As usual, we need to also account for any potential errors. Let's check if the response - `newPet` - has an error property. If so, we'll throw a new `Error` and pass it to the `catch` block.

```jsx
// src/App.jsx

  const handleAddPet = async (formData) => {
    try {
    const newPet = await petService.create(formData);

    if (newPet.err) {
      throw new Error(newPet.err);
    }

    setPets([newPet, ...pets]);
    setIsFormOpen(false);
    } catch (err) {
      // Log the error to the console
      console.log(err);
    }
  };
```

Test your form in the browser and ensure you can create a new pet and see it displayed in the UI.

## Take a step back

Let's review the big picture. Here's what happens when the user hits submit:

- The `formData` object gets passed from the `PetForm` component to the `App` component, where the `pets` state is held.
- The `App` component passes the `formData` to the `create()` service function.
- In the `create()` function, the `formData` is converted to JSON, sent to our route on the back-end, used (as `req.body`) to create a new pet, and returned as a new JSON pet object by the controller.
- The `create()` function converts the JSON pet object to useable JavaScript and returns it to the `App` component's `handleAddPet()` function.
- Our `pets` state is then set to a new array with our `newPet` and all the existing `pets`, updating it.
- Once our state updates, React refreshes the UI, and the user sees their brand new pet appear in the `PetList` component.

> 😎 Note that how the data flows is in the same order we constructed our code. This lets us more easily trace the data through our application and test as we go, so problems are more easily debugged, even if it means we're working a little slower along the way.

If that feels confusing, take a second to trace the data through your code! It's essential to visualize the flow of data when working with CRUD apps; now that we're passing data in both directions, this is an excellent time to practice this skill.
