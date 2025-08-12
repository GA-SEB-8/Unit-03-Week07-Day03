<h1>
  <span class="headline">Jukebox Back-End Lab</span>
  <span class="subhead">Exercise</span>
</h1>

## Requirements

Welcome to the Reactville Jukebox! In this lab, you'll build the back-end of a collaborative, community-driven jukebox application where anyone can contribute tunes by adding, updating, or deleting music tracks. This lab is designed to give you practice developing a back-end API using Express.

As you develop each feature, you should use Postman to test your API endpoints.

You'll implement full CRUD operations, allowing users to interact freely with the music database. Let's dive in and make Reactville a bit more musical!

### Technical stack

- **Node.js and Express**: The back-end will be built using Node.js with Express to handle server-side logic and HTTP requests.
- **MongoDB and Mongoose**: Use MongoDB as the database for storing track data, with Mongoose in your Node app to provide schema validation.

### Necessary middleware

- **`express.json()` middleware**: Implement Express' built-in `express.json()` middleware to parse incoming requests with JSON payloads.

### API requirements

- **One model - full CRUD**: The application will manage a single model, `Track`, which will support full CRUD operations.
- **Asynchronous processing**: All routes must utilize `async`/`await` to handle asynchronous operations properly.
- **Error handling**: Each route must include `try`/`catch` blocks to catch and handle errors effectively.

## Track model entity

| Field  | Type   | Required | Description                                 |
| ------ | ------ | -------- | ------------------------------------------- |
| title  | String | Yes      | The title of the track.                     |
| artist | String | Yes      | The artist of the track.                    |

## Required routes

| HTTP Method | Controller | Response | URI           | Use Case           |
| ----------- | ---------- | -------- | ------------- | ------------------ |
| POST        | `create`   | 201      | `/tracks`     | Create a track     |
| GET         | `index`    | 200      | `/tracks`     | List all tracks    |
| GET         | `show`     | 200      | `/tracks/:id` | Get a single track |
| PUT         | `update`   | 200      | `/tracks/:id` | Update a track     |
| DELETE      | `delete`   | 200      | `/tracks/:id` | Delete a track     |

### 1. Add a new track

- **Endpoint**: `POST /tracks`
- **Function**: Adds a new track to the database.
- **Success Status Code**: 201 Created
- **Error Status Code**: 500 Internal Server Error
- **Request Body**:

  ```json
  {
    "title": "Track Title",
    "artist": "Artist Name",
  }
  ```

### 2. Get all tracks

- **Endpoint**: `GET /tracks`
- **Function**: Retrieves a list of all tracks.
- **Response**: An array of track objects.
- **Success status code**: 200 OK
- **Error status code**: 500 Internal Server Error

### 3. Get a single track

- **Endpoint**: `GET /tracks/:id`
- **Function**: Retrieves details of a specific track using its ID.
- **Response**: A single track object.
- **Success status code**: 200 OK
- **Error status code**: 500 Internal Server Error

### 4. Update a track

- **Endpoint**: `PUT /tracks/:id`
- **Function**: Updates an existing track.
- **Success status code**: 200 OK
- **Error status code**: 500 Internal Server Error
- **Request body**:

  ```json
  {
    "title": "New Track Title",
    "artist": "New Artist Name",
  }
  ```

### 5. Delete a Track

- **Endpoint**: `DELETE /tracks/:id`
- **Function**: Deletes a track from the database.
- **Response**: The deleted track.
- **Success status code**: 200 OK
- **Error status code**: 500 Internal Server Error

Happy Coding!
