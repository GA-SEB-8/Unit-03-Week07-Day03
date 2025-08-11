<h1>
  <span class="headline">Jukebox Front-End Lab</span>
  <span class="subhead">Exercise</span>
</h1>

## Introduction

Welcome to the Reactville Jukebox! In this lab, you'll create the front-end of a collaborative, community-driven jukebox application. You have the option to design the user interface either as a dynamic dashboard with conditional rendering or by using React Router to manage different views for each function. Additionally, you'll complete the jukebox vibe by implementing a feature to display the currently playing track.

This lab is designed to give you practical experience in building a single-page application (SPA) using React and fetch to interact with the RESTful API you developed in the [Express API - Jukebox Back-End Lab](https://git.generalassemb.ly/modular-curriculum-all-courses/express-api-jukebox-back-end-lab). As you build out the functions for creating, reading, updating, and deleting (CRUD) tracks, you'll see how your front-end interacts seamlessly with the back-end, bringing the digital jukebox to life.

## Minimum requirements

You have the power to decide how to implement the UI in this application. Depending on your chosen UI structure, the requirements will slightly differ. Below are the functionalities your application must support, with notes for both UI approaches and the new requirement for tracking the current track. ***Choose only one path!***:

| Feature                     | Dashboard Requirements                                                                                                                                   | React Router Requirements                                                                                         |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Read all tracks**         | - Build a `TrackList` component.<br>- This component will be a child of the `App` component and should always be visible.<br>- Display the details of every track in the database in this component. | - Build a `Home` component.<br>- The `Home` component will be a child of the `App` component and be shown on the `/` route.<br>- Build a `TrackList` component.<br>- The `TrackList` component will be a child of the `Home` component.<br>- Display the details of every track in the database using the `TrackList` component. |
| **Create a new track**      | - Build a `TrackForm` component.<br>- This component will be a child of the `App` component.<br>- It should only be visible when a user clicks an **Add New Track** button above the `TrackList` component inside the `App` component.<br>- This component will display a form allowing users to add a track.<br>- When the user submits the form, the track should be added to the database. They should also see the new track in the `TrackList` component, and the `TrackForm` component should be hidden. | - Build a `TrackForm` component.<br>- This component will be a child of the `App` component and be shown on the `/add-track` route.<br>- The user should be able to navigate to this route by clicking an **Add New Track** button above the `TrackList` component inside the `Home` component.<br>- This component will display a form allowing users to add a track.<br>- When the user submits the form, the track should be added to the database, and they should automatically be redirected to the `/` route where they will see the new track. |
| **Update a track**          | - Add an **Edit** button under each track in the `TrackList` component to allow the user to select a track to edit. When the user selects a track to edit, the existing `TrackFrom` component should be shown.<br>- Refactor the `TrackForm` component so that it can also be used to edit an existing track.<br>- When the user submits the form to edit an existing track, it should be updated in the database. They should also see the updated track in the `TrackList` component, and the `TrackForm` component should be hidden. | - Add a `/edit-track/:trackId` route, which will show the existing `TrackForm` component.<br>- Add an **Edit** button under each track in the `TrackList` component allowing a user to navigate to this route. Ensure that the unique id for a track is passed in the URL.<br>- Refactor the `TrackForm` component so that it can also be used to edit an existing track.<br>- When the user submits the form to edit an existing track, it should be updated in the database, and they should automatically be redirected to the `/` route where they will see the updated track. |
| **Delete a track**          | - In the `TrackList` component, add a **Delete** button under each track.<br>- When the user clicks this button, the corresponding track should be deleted from the database and should no longer be shown in the `TrackList` component. | - In the `TrackList` component, add a **Delete** button under each track.<br>- When the user clicks this button, the corresponding track should be deleted from the database and should no longer be shown in the `TrackList` component. |
| **Currently playing track** | - Build a `NowPlaying` component.<br>- This component will be a child of the `App` component, placed after the `TrackList` component.<br>- In the `TrackList` component, add a **Play** button under each track.<br>- When a user clicks a **Play** button, display the details for the corresponding track as in the `NowPlaying` component.<br>- The `NowPlaying` component should not be visible if no track is playing. | - Build a `NowPlaying` component.<br>- This component will be a child of the `Home` component, placed after the `TrackList` component.<br>- In the `TrackList` component, add a **Play** button under each track.<br>- When a user clicks a **Play** button, display the details for the corresponding track as in the `NowPlaying` component.<br>- The `NowPlaying` component should not be visible if no track is playing. |

> ⚠️ Please note that this lab does not involve actual audio playback capabilities. It is designed as a UI simulation of a jukebox, focusing on the interaction with a music track database. The features implemented will simulate managing a playlist but will not play music.

## Component hierarchy

You can choose between maintaining a single-page flow with components that appear and disappear based on user actions or implementing routing to navigate between different components representing pages. Here is an example structure for each option:

### Dashboard style

- **App**
  - **TrackList**
  - **TrackForm** (conditionally rendered)
  - **NowPlaying** (conditionally rendered)

### React Router style

- **App**
  - **Home** (route: `/`)
    - **TrackList**
    - **NowPlaying** (conditionally rendered)
  - **TrackForm** (route: `/add-track` AND `/edit-track/:trackId`)

This lab offers you a chance to practice making design decisions based on user experience and functionality requirements. Whichever approach you choose, the key is ensuring the application remains user-friendly and performs full CRUD functionality paired with the Express API.
