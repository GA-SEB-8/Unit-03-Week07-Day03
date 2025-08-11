<h1>
  <span class="headline">Jukebox Front-End Lab</span>
  <span class="subhead">Setup</span>
</h1>

## Setup

Open your Terminal application and navigate to your `~/code/ga/labs` directory:

```bash
cd ~/code/ga/labs
```

Create a new Vite project for your React app:

```bash
npm create vite@latest
```

You'll be prompted to choose a project name. Name it `react-jukebox-front-end-lab`.

- Select a framework. Use the arrow keys to choose the `React` option and hit `Enter`.

- Select a variant. Again, use the arrow keys to choose `JavaScript` and hit `Enter`.

Navigate to your new project directory and install the necessary dependencies:

```bash
cd react-jukebox-front-end-lab
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

To start the development server and view the app in the browser, use the following command:

```bash
npm run dev
```

You should see that `Vite` is available on port number 5173:

```plaintext
localhost:5173
```

### Create file structure

Now setup the file and folder structure for your project:

- Create a new folder called `components` in the `src` directory.
- Create a new folder called `services` in the `src` directory.
- Inside of `services` create a new file called `trackService.js`.
- Inside of `trackService.js` add the base URL for your API.

### Using environment variables with Vite

In a React single-page application, we avoid hard-coding backend URLs so that we can easily switch settings between development and production environments. We use environment variables to manage these settings in our project. With Vite, all environment variables must start with `VITE_` to be accessible in the frontend code.

Create a `.env` file in the root of your frontend project:

```bash
touch.env
```

Add your backend server URL to the `.env` file:

```plaintext
VITE_BACK_END_SERVER_URL='http://localhost:3000'
```

Ensure the `.env` file is not tracked by Git by adding it to a `.gitignore` file.

Use the environment variable in your fetch calls:

```js
const BASE_URL = `${import.meta.env.VITE_BACK_END_SERVER_URL}/tracks`;
```

### GitHub setup

To add this project to GitHub, initialize a Git repository:

```bash
git init
git add .
git commit -m "init commit"
```

Make a new repository on [GitHub](https://github.com/) named `react-jukebox-front-end-lab`.

Link your local project to your remote GitHub repo:

```bash
git remote add origin https://github.com/<github-username>/react-jukebox-front-end-lab.git
git push origin main
```

> 🚨 Do not copy the above command. It will not work. Your GitHub username will replace `<github-username>` (including the `<` and `>`) in the URL above.

## Running the Express backend

Before diving into our React app development, you'll need to ensure that the Express backend server is operational. This backend will handle requests from your React app. You will be using the back-end server you created in the `Express API - Jukebox Back-End Lab` lab as the API for this lesson.

Follow these steps to set up the server:

Open your Terminal application and navigate to your `~/code/ga/labs/express-api-jukebox-back-end-lab` directory:

```bash
cd ~/code/ga/labs/express-api-jukebox-back-end-lab
```

Once there, run your server with `nodemon`:

```bash
nodemon server.js
```
