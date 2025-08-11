<h1>
  <span class="headline">Pets Front-End</span>
  <span class="subhead">Environment Variables in Vite</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to use environment variables in a Vite project to keep the front-end and back-end loosely coupled.

### Using environment variables with Vite

With a SPA like React, we want the back-end and front-end to be as loosely coupled as possible. This means that instead of hard-coding our `localhost:3000` address into the front-end codebase for our server calls, we want to use an environment variable that can change depending on context.

That way, if we deploy an app, we only need to tell the React app where the back-end is located using an environment variable.

So, we need to create a `.env` file on the front-end that will contain our server URL:

```bash
touch .env
```

Add the following:

```plaintext
VITE_BACK_END_SERVER_URL=http://localhost:3000
```

If this variable name seems awkward, it's because [Vite's environment variables](https://vitejs.dev/guide/env-and-mode) must start with `VITE_` to be read by the client application.

Also, be sure to add `.env` to the list in your `.gitignore` file if it's not there already so that our local version doesn't get pushed to GitHub.

Now, we can access this variable for use in all fetch calls to the server. For example, in `petService.js`, we'll add the following:

```javascript
// src/services/petService.js

const BASE_URL = `${import.meta.env.VITE_BACK_END_SERVER_URL}/pets`;
```

> 🚨 Environment variables are loaded when the app is first run, and Vite does not track changes to the `.env` file. If you alter your `.env` file, you must restart your development server for the changes to take effect.

Quit the running development server and restart it with the following command:

```bash
npm run dev
```
