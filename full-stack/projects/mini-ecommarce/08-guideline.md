# Coding Guidelines Document

## Mini E-Commerce Project (Node.js + React)

# 1. Purpose of This Document

This document defines basic coding standards and principles for building the Mini E-Commerce system. The goal is to ensure:

- Code is easy to understand
- Features are implemented consistently
- Project remains maintainable
- Team members can collaborate easily

This is not an advanced architecture guide. It focuses on building a working system in a simple and structured way.

# 2. General Coding Principles

## 2.1 Keep It Simple (KISS Principle)

Keep the implementation as simple as possible.

- Avoid unnecessary complexity
- Do not introduce advanced patterns early
- Prefer straightforward solutions over optimized abstractions

If a solution is difficult to understand, it is probably too complex for this stage.

## 2.2 Do Not Repeat Yourself (DRY Principle)

Avoid duplicating logic.

- If the same logic is used in multiple places, extract it into a function
- Reusable logic should be written once and reused

However, do not over-apply DRY by creating unnecessary abstractions.

## 2.3 One Responsibility Principle

Each file or function should have one clear responsibility.

Examples:

- A controller should handle request and response only
- A function should perform a single logical task
- A component should represent one UI element or screen section

Avoid mixing unrelated logic in a single file.

## 2.4 Readability Over Clever Code

Code should be written for humans first.

- Use clear and meaningful variable names
- Avoid complex one-line logic
- Prefer clarity over shortcuts or tricks

Another developer should be able to understand the code easily without explanation.

## 2.5 Consistency Principle

Follow the same style and structure throughout the project.

- Use consistent naming conventions
- Keep similar patterns across modules
- Organize code in a predictable manner

Consistency is more important than personal preference.

# 3. Backend Guidelines (Node.js + Express)

## 3.1 Project Structure Rule

Backend should be organized by feature modules:

- Authentication
- Product
- Cart
- Order
- Payment
- Admin

Each module should include:

- Routes
- Controller
- (Optional) Model

Do not over-structure the project into unnecessary layers.

## 3.2 Controller Responsibility

Controllers should:

- Receive request data
- Call required logic
- Send response

Controllers should NOT:

- Contain complex business logic
- Handle database design decisions
- Be responsible for unrelated operations

## 3.3 API Design Rules

- Use RESTful APIs
- Use standard HTTP methods (GET, POST, PUT, DELETE)
- Always return JSON responses

Standard response format:

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {}
}
```

## 3.4 Authentication Rules

- Use JWT for authentication
- Store user role (Customer / Admin)
- Protect admin-only routes using middleware

Passwords must always be stored in hashed form.

## 3.5 Error Handling

- Handle errors gracefully
- Do not crash the server due to user input
- Return meaningful error messages

Common errors:

- Unauthorized access
- Invalid input
- Resource not found
- Server error

## 3.6 Validation Rule

- Validate all input on backend
- Do not trust frontend data
- Ensure required fields are present before processing

# 4. Frontend Guidelines (React)

## 4.1 Folder Structure Principle

Frontend should be organized into:

- Pages (screens)
- Components (reusable UI elements)
- API layer (backend communication)

Do not create deep or unnecessary folder structures.

## 4.2 Pages vs Components Rule

- Pages represent full screens (e.g., Home, Cart, Checkout)
- Components represent reusable UI parts (e.g., Button, ProductCard)

Do not mix page logic inside components.

## 4.3 API Handling Rule

- All API calls must be written in a separate API layer
- Do not write fetch/axios calls directly inside components

This ensures separation of UI and data logic.

## 4.4 State Management Rule

- Use simple React state (`useState`, `useEffect`)
- Avoid introducing advanced state libraries in the first project

Only introduce complexity when required.

## 4.5 Component Design Rule

Components should:

- Be small and reusable
- Focus on a single UI responsibility
- Avoid containing business logic

# 5. Feature Development Guidelines

## 5.1 Build Incrementally

Follow this order:

1. Authentication (Login/Register)
2. Product listing
3. Product details
4. Cart management
5. Order creation
6. Payment integration
7. Admin dashboard

Do not jump between features randomly.

## 5.2 Feature Isolation Rule

Each feature should be built independently:

Example:

- Product module should work without Cart
- Cart should work without Payment

This ensures easier debugging and development.

## 5.3 Integration Rule

Once individual modules work, connect them step by step.

Avoid building everything together at the start.

# 6. Git and Version Control Guidelines

- Commit frequently with small changes
- Use clear commit messages

Examples:

- add product API
- implement login functionality
- fix cart quantity issue

Avoid large, unclear commits.

# 7. Naming Conventions

- Variables: camelCase
- Functions: camelCase
- Components: PascalCase
- Files: consistent and descriptive names

Avoid vague names like `data1`, `temp`, or `test`.

# 8. Final Development Principle

The main goal of this project is:

- To understand full-stack flow
- To learn how frontend and backend interact
- To build a working system end-to-end

Do not focus on advanced architecture or optimization in the first version. Focus on correctness, clarity, and completion.
