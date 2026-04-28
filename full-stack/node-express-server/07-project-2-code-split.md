In your `package.json`, add:

```json id="esm1"
{
  "type": "module"
}
```

# 0. Starting point (what you already have)

You currently have everything inside:

```js
server.js;
```

- Express app
- Routes
- Logic
- Data

We are going to split it into layers.

# 1. Create the folder structure

Inside your project:

Final structure:

```id="m1"
task-manager/
│
├── server.js
├── app.js
│
├── routes/
│   └── taskRoutes.js
│
├── controllers/
│   └── taskController.js
│
└── data/
    └── tasks.js
```

# 2. Step 1 — Move data to its own file

## data/tasks.js

```js id="d1"
let tasks = [];

export default tasks;
```

### What changed?

- You extracted storage
- Now data is shared using ES6 `export default`

Think:

> “Central storage file”

# 3. Step 2 — Create controllers (business logic)

## controllers/taskController.js

```js id="c1"
import tasks from "../data/tasks.js";

/*
CREATE TASK
*/
export const createTask = (req, res) => {
  const { title } = req.body;

  if (!title || title.trim() === "") {
    return res.status(400).json({ error: "Task title is required" });
  }

  const newTask = {
    id: Date.now(),
    title,
    completed: false,
    createdAt: new Date(),
  };

  tasks.push(newTask);

  res.status(201).json(newTask);
};

/*
GET ALL TASKS
*/
export const getAllTasks = (req, res) => {
  res.json(tasks);
};

/*
GET COMPLETED
*/
export const getCompletedTasks = (req, res) => {
  res.json(tasks.filter((t) => t.completed));
};

/*
GET PENDING
*/
export const getPendingTasks = (req, res) => {
  res.json(tasks.filter((t) => !t.completed));
};

/*
UPDATE TASK
*/
export const markCompleted = (req, res) => {
  const id = Number(req.params.id);

  const task = tasks.find((t) => t.id === id);

  if (!task) {
    return res.status(404).json({ error: "Task not found" });
  }

  task.completed = true;
  task.updatedAt = new Date();

  res.json(task);
};

/*
DELETE TASK
*/
export const deleteTask = (req, res) => {
  const id = Number(req.params.id);

  const index = tasks.findIndex((t) => t.id === id);

  if (index === -1) {
    return res.status(404).json({ error: "Task not found" });
  }

  tasks.splice(index, 1);

  res.json({ message: "Task deleted" });
};
```

## What changed here?

Before:

- Logic was inside server file

Now:

- All logic is centralized

Think:

> “Brain of application”

# 4. Step 3 — Create routes

## routes/taskRoutes.js

```js id="r1"
import express from "express";
const router = express.Router();

import {
  createTask,
  getAllTasks,
  getCompletedTasks,
  getPendingTasks,
  markCompleted,
  deleteTask,
} from "../controllers/taskController.js";

/*
ROUTES
*/

router.post("/tasks", createTask);
router.get("/tasks", getAllTasks);
router.get("/tasks/completed", getCompletedTasks);
router.get("/tasks/pending", getPendingTasks);
router.patch("/tasks/:id", markCompleted);
router.delete("/tasks/:id", deleteTask);

export default router;
```

## What changed here?

Before:

- Routes + logic mixed

Now:

- Routes only define URLs
- They just call controller functions

Think:

> “Traffic manager”

# 5. Step 4 — Create app setup

## app.js

```js id="a1"
import express from "express";
const app = express();

import taskRoutes from "./routes/taskRoutes.js";

// middleware
app.use(express.json());

// routes
app.use("/", taskRoutes);

export default app;
```

## What changed here?

This file:

- Creates Express app
- Adds middleware
- Connects routes

Think:

> “Configuration center”

# 6. Step 5 — Start server

## server.js

```js id="s1"
import app from "./app.js";

const PORT = 3000;

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

## What changed here?

This is now VERY clean:

- Only starts server
- Nothing else

Think:

> “Power button”

# 7. Final flow (very important)

Now request flow looks like this:

```id="flow"
Client
  ↓
server.js (starts app)
  ↓
app.js (Express setup)
  ↓
routes/taskRoutes.js (URL matching)
  ↓
controllers/taskController.js (logic)
  ↓
data/tasks.js (storage)
  ↓
Response
```

# 8. What you just learned (big picture)

You moved from:

### BEFORE

- One messy file
- Everything mixed

### AFTER

- Clean architecture
- Separation of concerns
- Real backend structure

# 9. Why this matters in real companies

This is exactly how production systems are built:

- Teams work independently
- Features are added safely
- Bugs are easier to trace
- Code scales without breaking

# 10. What you should feel now

You should now understand:

- Why folders exist
- Why logic is separated
- How request flows through system
- Why Express is structured this way
