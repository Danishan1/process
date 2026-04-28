# 1. Enable ES Modules in Node.js

In your `package.json`, add:

```json id="esm1"
{
  "type": "module"
}
```

This tells Node.js to use ES6 imports instead of CommonJS (`require`).

# 2. Final ES6 folder structure (same as before)

```id="fs1"
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
├── db/
│   └── connection.js
```

# 3. Database connection (ES6 version)

## db/connection.js

```js id="dbes1"
import mysql from "mysql2";

// Create connection
const db = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "your_password",
  database: "task_manager",
});

// Connect
db.connect((err) => {
  if (err) {
    console.error("Database connection failed:", err);
    return;
  }
  console.log("Connected to MySQL");
});

export default db;
```

# 4. Controller (modern async/await style)

Now we modernize the biggest part.

## controllers/taskController.js

```js id="ctrl2"
import db from "../db/connection.js";

/*
CREATE TASK
*/
export const createTask = (req, res) => {
  const { title } = req.body;

  if (!title) {
    return res.status(400).json({ error: "Title is required" });
  }

  const sql = "INSERT INTO tasks (title) VALUES (?)";

  db.query(sql, [title], (err, result) => {
    if (err) {
      return res.status(500).json(err);
    }

    res.status(201).json({
      id: result.insertId,
      title,
      completed: false,
    });
  });
};

/*
GET ALL TASKS
*/
export const getAllTasks = (req, res) => {
  const sql = "SELECT * FROM tasks";

  db.query(sql, (err, results) => {
    if (err) return res.status(500).json(err);

    res.json(results);
  });
};

/*
GET COMPLETED
*/
export const getCompletedTasks = (req, res) => {
  const sql = "SELECT * FROM tasks WHERE completed = true";

  db.query(sql, (err, results) => {
    if (err) return res.status(500).json(err);

    res.json(results);
  });
};

/*
GET PENDING
*/
export const getPendingTasks = (req, res) => {
  const sql = "SELECT * FROM tasks WHERE completed = false";

  db.query(sql, (err, results) => {
    if (err) return res.status(500).json(err);

    res.json(results);
  });
};

/*
MARK COMPLETE
*/
export const markCompleted = (req, res) => {
  const { id } = req.params;

  const sql = "UPDATE tasks SET completed = true WHERE id = ?";

  db.query(sql, [id], (err) => {
    if (err) return res.status(500).json(err);

    res.json({ message: "Task completed" });
  });
};

/*
DELETE TASK
*/
export const deleteTask = (req, res) => {
  const { id } = req.params;

  const sql = "DELETE FROM tasks WHERE id = ?";

  db.query(sql, [id], (err) => {
    if (err) return res.status(500).json(err);

    res.json({ message: "Task deleted" });
  });
};
```

# 5. Routes (ES6 clean imports)

## routes/taskRoutes.js

```js id="routes2"
import express from "express";

import {
  createTask,
  getAllTasks,
  getCompletedTasks,
  getPendingTasks,
  markCompleted,
  deleteTask,
} from "../controllers/taskController.js";

const router = express.Router();

router.post("/tasks", createTask);
router.get("/tasks", getAllTasks);
router.get("/tasks/completed", getCompletedTasks);
router.get("/tasks/pending", getPendingTasks);
router.patch("/tasks/:id", markCompleted);
router.delete("/tasks/:id", deleteTask);

export default router;
```

# 6. App setup (modern ES6)

## app.js

```js id="app2"
import express from "express";
import taskRoutes from "./routes/taskRoutes.js";

const app = express();

app.use(express.json());

app.use("/", taskRoutes);

export default app;
```

# 7. Server entry point

## server.js

```js id="server2"
import app from "./app.js";

const PORT = 3000;

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

# 8. What changed with ES6?

## Before (CommonJS)

```js id="old2"
const express = require("express");
module.exports = router;
```

## Now (ES6)

```js id="new2"
import express from "express";
export default router;
```

# 9. Why ES6 matters in real projects

Modern backend systems use ES6 because:

- Cleaner syntax
- Better modular design
- Standard in frontend + backend
- Works well with TypeScript
- Used in modern frameworks

# 10. Final architecture (clean mental model)

```id="flow3"
Client
  ↓
server.js
  ↓
app.js
  ↓
routes/
  ↓
controllers/
  ↓
MySQL database
```

# 11. What you just achieved

You now have:

- Real backend architecture
- MySQL integration
- ES6 module system
- REST API design
- Clean separation of concerns

This is **exactly how production Node.js APIs are structured**.

# 12. Next upgrade path (important)

If you continue, next serious step is:

### 1. Replace callbacks with async/await MySQL

(`mysql2/promise`)

### 2. Add environment variables (`dotenv`)

### 3. Add authentication (JWT login system)

### 4. Add ORM (Prisma or Sequelize)
