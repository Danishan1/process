# Project: Simple Task Manager API

This is similar to notes, but with a bit more structure:

- Create tasks
- Mark tasks as completed
- View pending/completed tasks
- Delete tasks

Everything stays in **one file**.

# 1. Setup

```bash
mkdir task-manager
cd task-manager
npm init -y
npm install express
```

Create:

```bash
server.js
```

# 2. Full Code (single file)

```js
// Import Express
const express = require("express");

// Create app
const app = express();

// Middleware
app.use(express.json());

// Port
const PORT = 3000;

// In-memory storage
let tasks = [];

/*
========================================
HOME ROUTE
========================================
*/
app.get("/", (req, res) => {
  res.send("Task Manager API is running");
});

/*
========================================
CREATE TASK
POST /tasks
========================================
*/
app.post("/tasks", (req, res) => {
  const { title } = req.body;

  if (!title || title.trim() === "") {
    return res.status(400).json({
      error: "Task title is required",
    });
  }

  const newTask = {
    id: Date.now(),
    title: title,
    completed: false,
    createdAt: new Date(),
  };

  tasks.push(newTask);

  res.status(201).json(newTask);
});

/*
========================================
GET ALL TASKS
GET /tasks
========================================
*/
app.get("/tasks", (req, res) => {
  res.json(tasks);
});

/*
========================================
GET ONLY COMPLETED TASKS
GET /tasks/completed
========================================
*/
app.get("/tasks/completed", (req, res) => {
  const completedTasks = tasks.filter((task) => task.completed);
  res.json(completedTasks);
});

/*
========================================
GET ONLY PENDING TASKS
GET /tasks/pending
========================================
*/
app.get("/tasks/pending", (req, res) => {
  const pendingTasks = tasks.filter((task) => !task.completed);
  res.json(pendingTasks);
});

/*
========================================
MARK TASK AS COMPLETED
PATCH /tasks/:id
========================================
*/
app.patch("/tasks/:id", (req, res) => {
  const id = Number(req.params.id);

  const task = tasks.find((t) => t.id === id);

  if (!task) {
    return res.status(404).json({
      error: "Task not found",
    });
  }

  task.completed = true;
  task.updatedAt = new Date();

  res.json(task);
});

/*
========================================
DELETE TASK
DELETE /tasks/:id
========================================
*/
app.delete("/tasks/:id", (req, res) => {
  const id = Number(req.params.id);

  const index = tasks.findIndex((t) => t.id === id);

  if (index === -1) {
    return res.status(404).json({
      error: "Task not found",
    });
  }

  tasks.splice(index, 1);

  res.json({
    message: "Task deleted successfully",
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

# 3. How to run

```bash
node server.js
```

# 4. Test using tools

Use:

- Postman
- Thunder Client

# 5. API Endpoints

## Create task

POST `/tasks`

```json
{
  "title": "Learn Express deeply"
}
```

## Get all tasks

GET `/tasks`

## Get completed tasks

GET `/tasks/completed`

## Get pending tasks

GET `/tasks/pending`

## Mark task as completed

PATCH `/tasks/:id`

## Delete task

DELETE `/tasks/:id`

# 6. What’s new compared to previous project

This one introduces:

- `PATCH` (partial update)
- Filtering data (`completed`, `pending`)
- Boolean field (`true/false`)
- Slightly more real-world logic

# 7. What you should notice

Focus on patterns:

- Same structure for every route
- Validation before logic
- Finding data using `find()`
- Updating objects directly
- Sending proper status codes
