Great—now we’ll actually build it step by step like a real backend project.

We’ll implement the **Blog API** using:

- Node.js
- Express
- MySQL
- ES6 modules
- Proper layered architecture (routes → controllers → models)

# 0. First: Create MySQL Database

Run this in MySQL:

```sql id="sql-blog-1"
CREATE DATABASE blog_api;

USE blog_api;
```

## Create tables

### Users table

```sql id="sql-blog-2"
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Posts table

```sql id="sql-blog-3"
CREATE TABLE posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT,
  title VARCHAR(255) NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

# 1. Install dependencies

```bash id="init-blog"
npm init -y
npm install express mysql2 dotenv
```

Enable ES6 in `package.json`:

```json id="esm-blog"
{
  "type": "module"
}
```

# 2. Final folder structure (we follow this exactly)

```id="structure-blog"
blog-api/
│
├── server.js
├── app.js
│
├── config/
│   └── db.js
│
├── routes/
│   ├── userRoutes.js
│   └── postRoutes.js
│
├── controllers/
│   ├── userController.js
│   └── postController.js
│
├── models/
│   ├── userModel.js
│   └── postModel.js
│
├── services/
│   └── postService.js
│
└── middleware/
    └── authMiddleware.js
```

# 3. Database connection

## config/db.js

```js id="db-blog"
import mysql from "mysql2";

const db = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "your_password",
  database: "blog_api",
});

db.connect((err) => {
  if (err) {
    console.error("DB connection failed:", err);
    return;
  }
  console.log("Connected to MySQL database");
});

export default db;
```

# 4. App setup

## app.js

```js id="app-blog"
import express from "express";
import userRoutes from "./routes/userRoutes.js";
import postRoutes from "./routes/postRoutes.js";

const app = express();

app.use(express.json());

app.use("/users", userRoutes);
app.use("/posts", postRoutes);

export default app;
```

# 5. Server entry

## server.js

```js id="server-blog"
import app from "./app.js";

const PORT = 3000;

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

# 6. USER MODULE

## 6.1 User Model (DB layer)

### models/userModel.js

```js id="user-model"
import db from "../config/db.js";

export const createUser = (name, email, callback) => {
  const sql = "INSERT INTO users (name, email) VALUES (?, ?)";
  db.query(sql, [name, email], callback);
};

export const getAllUsers = (callback) => {
  db.query("SELECT * FROM users", callback);
};
```

## 6.2 User Controller

### controllers/userController.js

```js id="user-controller"
import { createUser, getAllUsers } from "../models/userModel.js";

export const addUser = (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ error: "Name and email required" });
  }

  createUser(name, email, (err, result) => {
    if (err) return res.status(500).json(err);

    res.status(201).json({
      id: result.insertId,
      name,
      email,
    });
  });
};

export const fetchUsers = (req, res) => {
  getAllUsers((err, results) => {
    if (err) return res.status(500).json(err);

    res.json(results);
  });
};
```

## 6.3 User Routes

### routes/userRoutes.js

```js id="user-routes"
import express from "express";
import { addUser, fetchUsers } from "../controllers/userController.js";

const router = express.Router();

router.post("/", addUser);
router.get("/", fetchUsers);

export default router;
```

# 7. POST MODULE (core feature)

## 7.1 Post Model

### models/postModel.js

```js id="post-model"
import db from "../config/db.js";

export const createPost = (user_id, title, content, callback) => {
  const sql = "INSERT INTO posts (user_id, title, content) VALUES (?, ?, ?)";
  db.query(sql, [user_id, title, content], callback);
};

export const getAllPosts = (callback) => {
  const sql = `
    SELECT posts.*, users.name 
    FROM posts 
    JOIN users ON posts.user_id = users.id
  `;

  db.query(sql, callback);
};
```

## 7.2 Post Service (business logic layer)

### services/postService.js

```js id="post-service"
export const validatePost = (title, content) => {
  if (!title || !content) {
    return "Title and content required";
  }
  return null;
};
```

## 7.3 Post Controller

### controllers/postController.js

```js id="post-controller"
import { createPost, getAllPosts } from "../models/postModel.js";
import { validatePost } from "../services/postService.js";

export const addPost = (req, res) => {
  const { user_id, title, content } = req.body;

  const error = validatePost(title, content);
  if (error) {
    return res.status(400).json({ error });
  }

  createPost(user_id, title, content, (err, result) => {
    if (err) return res.status(500).json(err);

    res.status(201).json({
      id: result.insertId,
      user_id,
      title,
      content,
    });
  });
};

export const fetchPosts = (req, res) => {
  getAllPosts((err, results) => {
    if (err) return res.status(500).json(err);

    res.json(results);
  });
};
```

## 7.4 Post Routes

### routes/postRoutes.js

```js id="post-routes"
import express from "express";
import { addPost, fetchPosts } from "../controllers/postController.js";

const router = express.Router();

router.post("/", addPost);
router.get("/", fetchPosts);

export default router;
```

# 8. How the system works (important mental model)

## Example: Create Post

```http id="flow-blog-req"
POST /posts
```

Flow:

```id="flow-blog"
Client
 → routes/postRoutes.js
 → controllers/postController.js
 → services/postService.js
 → models/postModel.js
 → MySQL database
 → response
```

# 9. What you just built (real-world level)

You now have:

- REST API
- MySQL relational schema
- Join queries (users + posts)
- Service layer (business logic)
- MVC-style architecture
- Modular backend system

This is **production-style backend structure**.

# 10. What you should understand now

You should clearly see:

- Why controllers are NOT models
- Why services exist
- Why routes are thin
- Why DB logic is isolated
- Why real apps are structured this way
