# Mini E-Commerce System – Task Breakdown (8-Day Sprint Plan)

# Day 1: Project Foundation

## Sprint 1: Backend Setup

**T1.1** Initialize Node + Express project
**T1.2** Setup project folder structure (routes, controllers, models, middleware)
**T1.3** Configure environment variables (.env)
**T1.4** Setup Express server with basic health route

## Sprint 2: Frontend Setup

**T1.5** Initialize React app
**T1.6** Setup folder structure (pages, components, services, hooks)
**T1.7** Install routing system (React Router)
**T1.8** Setup base layout (Navbar + Routing shell)

## Sprint 3: Database Setup + Integration Check

**T1.9** Setup MySQL database connection in backend
**T1.10** Create initial database schema (empty structure setup)
**T1.11** Test backend ↔ database connection
**T1.12** Connect frontend to backend health API

# Day 2: Authentication System

## Sprint 1: Backend Auth

**T2.1** Create User table schema
**T2.2** Implement registration API
**T2.3** Implement login API
**T2.4** Implement password hashing (bcrypt)
**T2.5** Generate JWT token on login

## Sprint 2: Frontend Auth UI

**T2.6** Build Login page
**T2.7** Build Register page
**T2.8** Create Auth API service (axios wrapper)
**T2.9** Handle form validation (basic frontend validation)

## Sprint 3: Auth Integration

**T2.10** Store JWT in localStorage
**T2.11** Implement protected route logic
**T2.12** Create role-based redirect (Admin vs Customer)
**T2.13** Test full login/register flow

# Day 3: Product System (Core Backend + Admin UI)

## Sprint 1: Backend Product APIs

**T3.1** Create Product table schema
**T3.2** Create Add Product API
**T3.3** Create Get All Products API
**T3.4** Create Get Product By ID API

## Sprint 2: Admin Product UI

**T3.5** Build Admin Product Dashboard page
**T3.6** Build Add Product form UI
**T3.7** Connect Add Product API
**T3.8** Display product list in admin panel

## Sprint 3: Integration

**T3.9** Test product creation flow end-to-end
**T3.10** Fix API integration issues
**T3.11** Basic UI validation handling

# Day 4: Product CRUD + Categories

## Sprint 1: Backend Expansion

**T4.1** Create Update Product API
**T4.2** Create Delete Product API
**T4.3** Create Category table schema
**T4.4** Create Category CRUD APIs

## Sprint 2: Frontend Enhancement

**T4.5** Add Edit Product UI
**T4.6** Add Delete Product functionality
**T4.7** Add Category selection in product form

## Sprint 3: Integration Testing

**T4.8** Test full product CRUD lifecycle
**T4.9** Test category-product mapping
**T4.10** Fix inconsistencies and UI bugs

# Day 5: Customer Product Experience

## Sprint 1: Backend Search & Filter

**T5.1** Implement search API (by name)
**T5.2** Implement category filter API
**T5.3** Optimize product query responses

## Sprint 2: Frontend Customer UI

**T5.4** Build product listing page
**T5.5** Build product details page
**T5.6** Implement search UI
**T5.7** Implement filter UI

## Sprint 3: Integration

**T5.8** Connect product APIs to UI
**T5.9** Test browsing flow end-to-end
**T5.10** Fix UI responsiveness issues

# Day 6: Cart + Order System (Core Logic Day)

## Sprint 1: Cart System (Frontend)

**T6.1** Implement add to cart logic
**T6.2** Implement remove from cart
**T6.3** Implement quantity update
**T6.4** Store cart in localStorage or context

## Sprint 2: Order Backend

**T6.5** Create Order table schema
**T6.6** Create Order Items table schema
**T6.7** Implement Create Order API
**T6.8** Implement stock validation logic

## Sprint 3: Integration

**T6.9** Connect cart → order flow
**T6.10** Validate stock deduction
**T6.11** Test complete order creation flow

# Day 7: Orders + Payment System

## Sprint 1: Order APIs Expansion

**T7.1** Create Order History API
**T7.2** Create Order Detail API
**T7.3** Create Admin Order View API
**T7.4** Create Order Status Update API

## Sprint 2: Payment System

**T7.5** Create Payment Initialization API
**T7.6** Integrate payment gateway (Razorpay/Stripe/mock)
**T7.7** Create Payment Status API
**T7.8** Store payment records linked to orders

## Sprint 3: Integration

**T7.9** Connect checkout → payment → order flow
**T7.10** Handle success/failure scenarios
**T7.11** Test full checkout pipeline

# Day 8: Admin Dashboard + Final System

## Sprint 1: Admin Analytics Backend

**T8.1** Create Total Orders API
**T8.2** Create Revenue Calculation API
**T8.3** Create Top Products API
**T8.4** Create Low Stock Products API

## Sprint 2: Admin Dashboard UI

**T8.5** Build admin dashboard layout
**T8.6** Display analytics cards
**T8.7** Build order management UI
**T8.8** Build inventory view UI

## Sprint 3: Final Integration + Testing

**T8.9** Fix authentication edge cases
**T8.10** Fix UI/UX inconsistencies
**T8.11** End-to-end system testing
**T8.12** Prepare deployment-ready build

# Final Deliverable Checklist

After completion:

## Backend

- Auth system
- Product system
- Cart/order system
- Payment system
- Admin APIs

## Frontend

- Customer UI
- Admin dashboard
- Auth system
- Full checkout flow

## System

- Role-based access
- Payment integration
- Database relations working
- Fully functional e-commerce flow
