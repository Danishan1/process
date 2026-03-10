# 1️⃣ HTTP and React (Lecture 41)

### Q1. What is HTTP?

**HTTP** is a protocol used for **communication between a client (browser) and a server** to transfer data over the web.

### Q2. Why is HTTP used in React applications?

HTTP is used in React to:

- Fetch data from APIs
- Send user data to servers
- Communicate with backend services
- Build dynamic applications

### Q3. What is an API?

An **API (Application Programming Interface)** allows different applications to **communicate and exchange data**.

Example:
A React app fetching data from a **REST API**.

### Q4. What is a REST API?

**REST API** is a web service that follows REST principles and allows CRUD operations through HTTP methods.

### Q5. What are the common HTTP methods used in React?

| Method | Purpose       |
| ------ | ------------- |
| GET    | Retrieve data |
| POST   | Send data     |
| PUT    | Update data   |
| DELETE | Remove data   |

# 2️⃣ HTTP GET Request (Lecture 42)

### Q6. What is a GET request?

A **GET request** is used to **retrieve data from a server**.

Example: Fetching user data from an API.

### Q7. How can React perform HTTP requests?

React can perform HTTP requests using:

1. **Axios**
2. **Fetch API**

### Q8. What is Axios?

**Axios** is a popular library used to **make HTTP requests from the browser or Node.js**.

Features:

- Promise-based
- Automatic JSON parsing
- Error handling

### Q9. Why is Axios preferred over Fetch API?

Advantages of Axios:

- Simpler syntax
- Automatic JSON conversion
- Better error handling
- Request cancellation support

### Q10. Where should API calls be made in class components?

Usually inside:

```
componentDidMount()
```

Because it runs **after the component is mounted**.

### Q11. What happens when a GET request fails?

React applications should handle errors using:

- `.catch()` block
- Error state
- Error boundaries

# 3️⃣ HTTP POST Request (Lecture 43)

### Q12. What is a POST request?

A **POST request** is used to **send data to a server**.

Example:
Submitting a form.

### Q13. What type of data is usually sent in POST requests?

Common data formats:

- JSON
- Form data
- XML

Example JSON:

```
{
 "name": "John",
 "email": "john@email.com"
}
```

### Q14. What are common use cases of POST requests?

- User registration
- Login authentication
- Submitting forms
- Creating new database records

### Q15. What is the difference between GET and POST?

| Feature       | GET           | POST               |
| ------------- | ------------- | ------------------ |
| Purpose       | Retrieve data | Send data          |
| Data location | URL           | Request body       |
| Security      | Less secure   | More secure        |
| Size limit    | Limited       | Large data allowed |

### Q16. Why is POST more secure than GET?

Because POST data is sent in the **request body instead of the URL**, making it less visible.

# ⭐ Very Important Interview Questions (41–43)

Focus on these:

1. What is HTTP?
2. What is a REST API?
3. What is the difference between GET and POST?
4. What is Axios?
5. Axios vs Fetch API
6. Where should API calls be made in React?
7. What is an HTTP request in React?
8. Why are APIs used in React applications?
