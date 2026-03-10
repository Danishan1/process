# 1️⃣ HTTP and React (Basics Practice)

### Practice Tasks

1. Create a React component that **fetches data from an API** and displays it.
2. Install and configure **Axios** in a React project.
3. Create a component that **loads data when the page loads**.
4. Display **loading text** while data is being fetched.
5. Show an **error message if API fails**.

Example requirement:

```
Loading...
Users Loaded Successfully
Error: Unable to fetch data
```

# 2️⃣ HTTP GET Request (Lecture 42)

### Practice Questions

1. Fetch a list of **users from a public API** and display their:
   - name
   - email
   - city

2. Display **API data inside a table**.

3. Create a component that shows a **list of posts from an API**.

4. Use **Fetch API** to retrieve data.

5. Use **Axios** to fetch the same data.

6. Display data using **map() list rendering**.

Example practice API:

```
https://jsonplaceholder.typicode.com/users
```

### Additional Practice

7. Create a **refresh button** that reloads API data.
8. Display only the **first 5 API records**.
9. Create a **search filter** for fetched data.
10. Display a **loading spinner before API response**.

# 3️⃣ HTTP POST Request (Lecture 43)

### Practice Questions

1. Create a **form that sends data to an API using POST request**.

Form fields:

- name
- email
- phone

2. Submit form data to an API endpoint.

Example API:

```
https://jsonplaceholder.typicode.com/posts
```

3. Display **success message after POST request**.

Example:

```
User Added Successfully
```

4. Clear form inputs after submission.

### Additional Practice

5. Create a **user registration form** that sends data using POST request.

6. Display the **response returned by API**.

Example response:

```
{
 id:101,
 name:"John"
}
```

7. Handle **POST request errors**.

Example:

```
Failed to submit data
```

# 4️⃣ Combined Practice (GET + POST)

Try these **combined exercises**.

### Exercise 1 – Users Dashboard

Requirements:

- Fetch users from API
- Display them in a list
- Add new user using POST request

### Exercise 2 – Blog Posts Manager

Features:

- Fetch posts using GET
- Add new post using POST
- Display posts dynamically

### Exercise 3 – Todo List API

Features:

- Fetch todos from API
- Add new todo item
- Show success message after submission

# ⭐ Mini Projects Using Topics 41–43

### 1️⃣ User List App

Features:

- Fetch users from API
- Display user cards

### 2️⃣ Blog Post Creator

Features:

- Create post form
- Send POST request
- Show new post

### 3️⃣ Todo Manager

Features:

- GET todo list
- POST new todo

# ⭐ Important Practical Interview Tasks

You may be asked tasks like:

1. Fetch data from API and display it in React.
2. Create a form that submits data using POST request.
3. Display loading and error states during API calls.
4. Use Axios to make HTTP requests.
5. Render API data dynamically using `map()`.
