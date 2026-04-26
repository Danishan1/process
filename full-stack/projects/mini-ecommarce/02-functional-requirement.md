# Functional Requirements Specification (FRS)

## Project: Mini E-Commerce System with Admin Dashboard

# 1. User Roles and Responsibilities

## 1.1 Customer Role

The Customer is an end-user who interacts with the platform to browse products and place orders.

### Responsibilities:

- Register and maintain personal account
- Browse and search products
- Manage shopping cart
- Place orders
- Make payments
- View order history
- Track order status
- Manage basic profile information (optional)

## 1.2 Admin Role

The Admin is a system manager responsible for maintaining products, users, orders, and business operations.

### Responsibilities:

- Manage product catalog (CRUD operations)
- Manage inventory and stock levels
- Manage orders and update order statuses
- View customer data (limited to operational needs)
- Manage payment records and reconciliation status
- View analytics and reports
- Manage categories and product structure
- Handle system-level monitoring (basic logs or activity tracking)

# 2. Authentication and Authorization

## FR-1: User Registration

- System shall allow customers to register using name, email, and password.
- System shall validate email uniqueness.
- System shall store passwords in hashed form.

## FR-2: User Login

- System shall allow login using email and password.
- System shall generate a JWT token on successful authentication.
- System shall reject invalid credentials.

## FR-3: Role-Based Access Control

- System shall assign roles: Customer or Admin.
- System shall restrict API access based on role.
- Admin-only endpoints shall not be accessible to customers.

## FR-4: Session Management

- System shall maintain authentication state using JWT.
- System shall allow logout by clearing client session.

# 3. Product Management (Admin Module)

## FR-5: Create Product

- Admin shall be able to add products with:
  - Name
  - Description
  - Price
  - Stock quantity
  - Category
  - Image URL

## FR-6: Update Product

- Admin shall be able to modify product details.
- System shall validate updated data before saving.

## FR-7: Delete Product

- Admin shall be able to remove products permanently.

## FR-8: Manage Product Stock

- Admin shall be able to update stock levels manually.
- System shall automatically reduce stock after order confirmation.

## FR-9: Product Categorization

- Admin shall be able to create and manage product categories.
- Products must belong to at least one category.

## FR-10: View Product Analytics

- Admin shall be able to view:
  - Most sold products
  - Low stock products
  - Product performance summary

# 4. Customer Product Access

## FR-11: View Products

- Customers shall be able to view all available products.

## FR-12: Product Search

- System shall allow search by product name.

## FR-13: Product Filtering

- System shall allow filtering by category and price range.

## FR-14: Product Details View

- System shall show detailed product information.

# 5. Cart Management

## FR-15: Add to Cart

- Customers shall be able to add products to cart.

## FR-16: Update Cart

- Customers shall be able to update product quantity in cart.

## FR-17: Remove from Cart

- Customers shall be able to remove items from cart.

## FR-18: Cart Storage

- System shall persist cart either in local storage or database.

# 6. Order Management

## FR-19: Order Creation

- System shall allow customers to place orders from cart.

## FR-20: Order Validation

- System shall validate stock availability before order confirmation.

## FR-21: Order Record Creation

- System shall create:
  - Order record
  - Order items records

- System shall associate order with user.

## FR-22: Order History (Customer)

- Customers shall be able to view past orders.

## FR-23: Order Details

- Customers shall be able to view full order details.

# 7. Payment System

## FR-24: Payment Initialization

- System shall initiate payment during checkout.

## FR-25: Payment Gateway Integration

- System shall integrate with a payment gateway (e.g., Stripe or Razorpay mock integration).
- System shall support at least:
  - Card payments
  - UPI or wallet simulation (optional for demo)

## FR-26: Payment Status Handling

System shall maintain payment statuses:

- Pending
- Success
- Failed

## FR-27: Payment Confirmation

- System shall confirm order only after successful payment.

## FR-28: Payment Failure Handling

- System shall allow retry in case of failed payments.
- System shall not create confirmed orders for failed transactions.

## FR-29: Payment Records

- System shall store payment details linked to orders:
  - Transaction ID
  - Payment method
  - Status
  - Timestamp

# 8. Admin Order Management

## FR-30: View All Orders

- Admin shall be able to view all customer orders.

## FR-31: Update Order Status

- Admin shall be able to update order status:
  - Pending
  - Processing
  - Shipped
  - Delivered
  - Cancelled

## FR-32: Order-Payment Mapping

- Admin shall be able to view payment status for each order.

## FR-33: Refund Handling (Optional Extension)

- Admin shall be able to mark orders as refunded.
- System shall record refund status.

# 9. Admin Dashboard and Analytics

## FR-34: Sales Overview

- Admin shall be able to view total revenue.

## FR-35: Order Analytics

- System shall show:
  - Total orders
  - Orders by status
  - Daily/weekly sales trends

## FR-36: User Analytics

- Admin shall be able to view:
  - Total registered users
  - Active users (optional)

## FR-37: Product Performance Dashboard

- System shall display:
  - Top-selling products
  - Low-performing products

## FR-38: Inventory Alerts

- System shall highlight low-stock products.

# 10. Customer Profile Management

## FR-39: Profile View

- Customer shall be able to view profile information.

## FR-40: Profile Update

- Customer shall be able to update:
  - Name
  - Password

# 11. System Validation and Error Handling

## FR-41: Input Validation

- System shall validate all user inputs on backend.

## FR-42: Error Handling

- System shall return structured error messages for:
  - Unauthorized access
  - Invalid input
  - Resource not found
  - Payment failure

## FR-43: Stock Validation

- System shall prevent order placement if stock is insufficient.

# 12. API Communication Requirements

## FR-44: REST API Design

- System shall expose RESTful APIs for all modules:
  - Auth
  - Products
  - Cart
  - Orders
  - Payments

## FR-45: JSON Communication

- System shall use JSON format for all API requests and responses.

# Summary of Responsibilities

## Customer Handles:

- Account creation and login
- Product browsing and search
- Cart management
- Order placement
- Payment processing
- Order tracking
- Profile updates

## Admin Handles:

- Product lifecycle management
- Inventory control
- Order management and status updates
- Payment tracking and reconciliation
- Business analytics and reporting
- Category management
- System oversight
