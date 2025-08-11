<h1>
  <span class="headline">Pets Back-End</span>
  <span class="subhead">Defining the Database</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to define and export a mongoose model for use with an API.

## Create the model file `pet.js`

While this application will not have multiple models, it's still a good idea to create a directory called models that will store all Mongoose models used in the application. Let's go ahead and create the directory, along with our `pet.js` file inside of it:

```bash
mkdir models
touch models/pet.js
```

> 💡 We use a singular naming convention for model files, as a single file will always export just one model.

## Create the pet schema

Before we're able to define our model, we must first import the mongoose library into our `pet.js` file:

```js
// models/pet.js

const mongoose = require('mongoose');
```

Next we'll define the schema, which provides a consistent structure to our data.

In this case, our pet model will be pretty simple, with a `name` property of type `String`, an `age` property of type `Number`, and a `breed` property of type `String`. We'll also specify that `name` and `age` are required fields, and give `age` a minimum value of 0.

Here's how we'll define that in our `pet.js` file:

```js
// models/pet.js

const petSchema = mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  age: {
    type: Number,
    min: 0,
    required: true,
  },
  breed: String,
});
```

## Register the model

Now that we've defined our schema, we'll tell mongoose to create a collection in mongodb and validate that collection's data using the schema. We do this using the `mongoose.model` method:

```js
// models/pet.js
const Pet = mongoose.model('Pet', petSchema);
```

## Export the model from the pet.js file

Next, we'll export the `Pet` model we just created so that the rest of our application has access to it.

```js
// models/pet.js

module.exports = Pet;
```

Our finished `pet.js` file will look like this:

```js
// models/pet.js

const mongoose = require('mongoose');

const petSchema = mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  age: {
    type: Number,
    min: 0,
  },
  breed: String,
});

const Pet = mongoose.model('Pet', petSchema);

module.exports = Pet;
```

## Import the model

Finally, we'll need the `Pet` model in many of our routes, so let's import it next. Similarly to our `models` directory, we'll be using a `controllers` directory to help keep a clear separation of concerns. Let's create that next:

```bash
mkdir controllers
touch controllers/pets.js
```

In `controllers/pets.js`, so let's add an import statement to the top of that file:

```js
// controllers/pets.js
const Pet = require('../models/pet.js');
```

That's it! Our model is set up and ready to be used in the request handling functions defined in our routes.
