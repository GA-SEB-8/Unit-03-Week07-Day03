<h1>
  <span class="headline">Pets Back-End</span>
  <span class="subhead">Index Route</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create an index route to retrieve all pets from the database and send this data as a JSON response to the client.

## READ - Index Route

In this section, we will create a new route to find all pets. This route will be a `GET` request to `/pets`, and will return a JSON response with all the pets in the database.

We will be following these specs when building the route:

- **CRUD Action:** READ
- **Method:** `GET`
- **Path:** `/pets`
- **Response:** JSON
- **Success Status Code:** `200` Ok
- **Success Response Body:** An array of all the pets in the database named `pets`. The array will be empty if there are no pets in the database.
- **Error Status Code:** `500` Internal Server Error
- **Error Response Body:** A JSON object with an `err` key and a message describing the error.

Let's get started! 🎉

## Create a GET Route - Index

Inside `controllers/pets.js`, create a new route to find all pets. The **method** for this route is `GET`, and the **path** is `/pets`. Use the `.get()` method on the `router` object to create the route and pass in the path and a callback function.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
  // Setting up for our code
});
```

### Test the Route

Test the route before adding a query to the database to ensure it works. We can do this by sending a response message to the client.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
  // Add a message to test the route
  res.json({ message: 'Index Route' });
});
```

Let's use Postman to test the route. Open Postman and send a `GET` request to `http://localhost:3000/pets`. You should receive a JSON response with the message `Index Route`. If you do not receive this message, debug before moving on.

### Try - Find All Pets

We have tested the route, so let's query the database and find all pets. Inside our controller function, we'll call `find()` on our `Pet` model. This returns all pets in the database. We'll then send a JSON response to the client with the pets array.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
  try {
    const foundPets = await Pet.find();
    res.status(200).json(foundPets);  // 200 OK
  } catch (err) {
    // Add error handling code
  }
});
```

Be sure to include a status of `200 Ok` with your JSON response body.

### Catch - errors

Use the `err.message` object once again to send a meaningful response to the client. Include a `500` status to indicate that there was an internal error on the server side.

```js
router.get('/', async (req, res) => {
  try {
    const foundPets = await Pet.find();
    res.status(200).json(foundPets);
  } catch (err) {
    res.status(500).json({ err: err.message }); // 500 Internal Server Error
  }
});
```

## 🎓 You do: Test the Index route with Postman

Now that we have finished the route let's test it with Postman. Send a `GET` request to `http://localhost:3000/pets`. Test both **success** and **error** messages.

- **Success:** You should receive a JSON response with all the pets in the database.
- **Error:** You should receive a JSON response with an error message. This message should describe the error that has occurred.

Since we have yet to add the delete functionality, you might not be able to see an empty array response. This is okay; check the response's structure to ensure it is correct. When we add the delete functionality, you can test for the empty array response.

> 🧠 If you are unsure how to test the failure message here, you can simulate an error by creating one yourself. You can add a `throw` statement on the first line of the `try` block. For example, `throw new Error('This is an error message')`. This will throw an error, triggering the `catch` block. This is only for testing purposes and should be removed once you have tested the error message.
