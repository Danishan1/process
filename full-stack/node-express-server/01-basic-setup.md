# Simple Notes API using Node.js and Express

## 1. What you are building

You are creating a small backend server that:

- Stores notes in memory
- Lets users create, read, update, and delete notes (CRUD)
- Runs locally on your machine

This is how most real-world APIs start.

## 2. Project setup

Open your terminal and run:

```bash
mkdir notes-api
cd notes-api
npm init -y
npm install express
```

Now create a file:

```
server.js
```

## 3. Full beginner-friendly code (single file)

Copy this entire code into `server.js`:

```js
// Import Express
const express = require("express");

// Create app
const app = express();

// Middleware to read JSON
app.use(express.json());

// Port
const PORT = 3000;

// In-memory database (temporary)
let notes = [];

/*
========================================
ROOT ROUTE
========================================
*/
app.get("/", (req, res) => {
  res.send("Welcome to Notes API");
});

/*
========================================
CREATE NOTE
POST /notes
========================================
*/
app.post("/notes", (req, res) => {
  const { text } = req.body;

  // Validation
  if (!text || text.trim() === "") {
    return res.status(400).json({
      error: "Text is required",
    });
  }

  const newNote = {
    id: Date.now(),
    text: text,
    createdAt: new Date(),
  };

  notes.push(newNote);

  res.status(201).json(newNote);
});

/*
========================================
GET ALL NOTES
GET /notes
========================================
*/
app.get("/notes", (req, res) => {
  res.json(notes);
});

/*
========================================
GET SINGLE NOTE
GET /notes/:id
========================================
*/
app.get("/notes/:id", (req, res) => {
  const id = Number(req.params.id);

  const note = notes.find((n) => n.id === id);

  if (!note) {
    return res.status(404).json({
      error: "Note not found",
    });
  }

  res.json(note);
});

/*
========================================
UPDATE NOTE
PUT /notes/:id
========================================
*/
app.put("/notes/:id", (req, res) => {
  const id = Number(req.params.id);
  const { text } = req.body;

  const note = notes.find((n) => n.id === id);

  if (!note) {
    return res.status(404).json({
      error: "Note not found",
    });
  }

  if (!text || text.trim() === "") {
    return res.status(400).json({
      error: "Text is required",
    });
  }

  note.text = text;
  note.updatedAt = new Date();

  res.json(note);
});

/*
========================================
DELETE NOTE
DELETE /notes/:id
========================================
*/
app.delete("/notes/:id", (req, res) => {
  const id = Number(req.params.id);

  const index = notes.findIndex((n) => n.id === id);

  if (index === -1) {
    return res.status(404).json({
      error: "Note not found",
    });
  }

  notes.splice(index, 1);

  res.json({
    message: "Note deleted successfully",
  });
});

/*
========================================
START SERVER
========================================
*/
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

## 4. How to run the project

In terminal:

```bash
node server.js
```

Open browser:

```
http://localhost:3000
```

## 5. How to test the API

Use tools like:

- Postman
- Thunder Client

### Create a note

POST `/notes`

```json
{
  "text": "My first note"
}
```

### Get all notes

GET `/notes`

### Get single note

GET `/notes/:id`

Example:

```
/notes/123456
```

### Update note

PUT `/notes/:id`

```json
{
  "text": "Updated note"
}
```

### Delete note

DELETE `/notes/:id`

## 6. Key concepts explained

### Express app

```js
const app = express();
```

This creates your server.

### Middleware

```js
app.use(express.json());
```

Allows your server to read JSON from requests.

### Routes

```js
app.get('/notes', ...)
app.post('/notes', ...)
```

Each route handles a specific request.

### Request object (`req`)

Contains data from client:

- `req.body` → POST data
- `req.params` → URL parameters

### Response object (`res`)

Used to send data back:

- `res.json()` → send JSON
- `res.send()` → send text
- `res.status()` → set HTTP status

## 7. Limitations of this project

Important to understand:

- Data is stored in memory (lost when server restarts)
- No database
- No authentication
- Everything in one file (not scalable)

## 8. Next step (what we will do later)

Once you're comfortable, we will improve this in layers:

1. Split into multiple files (routes, controllers)
2. Add a real database (MongoDB)
3. Add environment variables
4. Add error handling middleware
5. Add authentication (login system)
