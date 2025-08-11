<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Setup</span>
</h1>

## Setup

Open your Terminal application and navigate to your `~/code/ga/lectures` directory:

```bash
cd ~/code/ga/lectures
```

Create a new Vite project for your React app:

```bash
npm create vite@latest
```

You'll be prompted to choose a project name. Let's name it `react-pets-front-end`.

- Select a framework. Use the arrow keys to choose the `React` option and hit `Enter`.

- Select a variant. Again, use the arrow keys to choose `JavaScript` and hit `Enter`.

Navigate to your new project directory and install the necessary dependencies:

```bash
cd react-pets-front-end
npm i
```

Open the project's folder in your code editor:

```bash
code .
```

### Configuring ESLint

Before we begin, we need to adjust the ESLint configuration. Add the indicated rules to the `rules` object in your `eslint.config.js` file:

```javascript
    rules: {
      ...js.configs.recommended.rules,
      ...react.configs.recommended.rules,
      ...react.configs['jsx-runtime'].rules,
      ...reactHooks.configs.recommended.rules,
      'react/jsx-no-target-blank': 'off',
      'react-refresh/only-export-components': [
        'warn',
        { allowConstantExport: true },
      ],
      'react/prop-types': 'off', // add this line
      'react/no-unescaped-entities': 'off', // add this line
    },
```

The first addition prevents warnings if you don't declare types for your props (which we're not), and the second prevents warnings if you use contractions within JSX.

### Clear `App.jsx`

Open the `App.jsx` file in the `src` directory and replace the contents of it with the following:

```jsx
// src/App.jsx

const App = () => {
  return <h1>Hello world!</h1>;
};

export default App;
```

### Running the development server

To start the development server and view our app in the browser, we'll use the following command:

```bash
npm run dev
```

You should see that `Vite` is available on port number 5173:

```plaintext
localhost:5173
```

### Create file structure

Now, let's set up the starting directory and file structure for our project:

```bash
mkdir src/components src/services
touch src/services/petService.js
```

We are going to keep all components we create inside the `components` directory. This will help us keep our project organized and easy to navigate.

`petService.js` will hold the base URL for our API and any functions that make calls to the server for pet data. This structure will help us keep our code easy to maintain.

### GitHub setup

To add this project to GitHub, initialize a Git repository:

```bash
git init
git add .
git commit -m "init commit"
```

Make a new repository on [GitHub](https://github.com/) named `react-pets-front-end`.

Link your local project to your remote GitHub repo:

```bash
git remote add origin https://github.com/<github-username>/react-pets-front-end.git
git push origin main
```

> 🚨 Do not copy the above command. It will not work. Your GitHub username will replace `<github-username>` (including the `<` and `>`) in the URL above.

## Running the Express back-end

Before diving into our React app development, you must ensure the Express back-end server is operational. This back-end will handle requests from your React app. You will be using the back-end server you created in the `Express API - Pets Back-End` lesson as the API for this lesson.

> 😎 If your `express-api-pets-back-end` is incomplete, you can obtain a fully implemented version from the [solution code repo](https://git.generalassemb.ly/modular-curriculum-all-courses/express-api-pets-back-end-solution). Install all the necessary dependencies with `npm i` and establish a connection to your MongoDB Atlas by adding a connection string in a `.env` file.

Follow these steps to set up the server:

Open your Terminal application and navigate to your `~/code/ga/lectures/express-api-pets-back-end` directory:

```bash
cd ~/code/ga/lectures/express-api-pets-back-end
```

Once there, run your server with `nodemon`:

```bash
nodemon
```

### Create your server `.env`

> 🧠 You may already have a `.env` file set up for your Express back-end already. If you do, skip this section.

To set up your server using the provided starter code, you'll need to create a `.env` file that includes a `MONGODB_URI` connection string. This connection string links your application to a MongoDB Atlas database. First, ensure you have a MongoDB Atlas account and create a database cluster.

Add a `.env` file to your server application and add the following secret key:

```plaintext
MONGODB_URI=your_mongodb_atlas_connection_string_here
```

Start the server, and you will be ready to start on the React front-end!

```bash
nodemon
```

### Ensure you have pets in your database

If you haven't created any pets or deleted the existing ones, you'll need new ones. You can add some new ones using Postman once your server is running. Make a `POST` request to `http://localhost:3000/pets` with a JSON object that looks like this in the body:

```json
{
  "name": "Fluffy",
  "breed": "Cat",
  "age": 3,
}
```
