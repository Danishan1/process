# UI Theme System (CSS Modules Based)

This will act as a **design contract** for all frontend work.

# 1. Design Philosophy

The UI must follow these principles:

- Clean and minimal (no clutter)
- Card-based layout for products and data
- Strong visual hierarchy (title → price → action)
- Consistent spacing and alignment
- Mobile-responsive by default
- No inline styles (CSS Modules only)

# 2. Global Design Tokens (Must Follow Everywhere)

Even though CSS Modules are used, we standardize values mentally and optionally in a `theme.css` or `variables.css`.

## 2.1 Color Palette

### Primary Colors

- Primary: `#2563eb` (blue)
- Primary Dark: `#1d4ed8`
- Accent: `#10b981` (green for success/actions)

### Neutral Colors

- Background: `#f9fafb`
- Card Background: `#ffffff`
- Border: `#e5e7eb`
- Text Primary: `#111827`
- Text Secondary: `#6b7280`

### Status Colors

- Success: `#16a34a`
- Warning: `#f59e0b`
- Error: `#ef4444`

## 2.2 Typography Rules

- Font family: `Inter, system-ui, sans-serif`
- Headings:
  - H1: 28–32px
  - H2: 22–26px
  - H3: 18–20px

- Body text: 14–16px
- Small text: 12px

Rules:

- No more than 2 font weights per page
- Avoid decorative fonts

## 2.3 Spacing System (Strict Rule)

Use only:

- 4px
- 8px
- 12px
- 16px
- 24px
- 32px
- 48px

Never use random spacing values.

## 2.4 Border Radius System

- Small: 4px (inputs, buttons)
- Medium: 8px (cards, modals)
- Large: 12–16px (containers)

## 2.5 Shadow System

- Light: `0 1px 3px rgba(0,0,0,0.1)`
- Medium: `0 4px 10px rgba(0,0,0,0.1)`
- Strong: `0 10px 25px rgba(0,0,0,0.15)`

# 3. Layout System Rules

## 3.1 Page Structure (Mandatory for all pages)

Every page must follow:

```
Navbar
Main Container
  Page Content
Footer (optional)
```

## 3.2 Container Rule

- Max width: `1200px`
- Center aligned
- Padding: `16px` (mobile), `24px` (desktop)

## 3.3 Grid System

For product listing:

- Desktop: 3–4 columns
- Tablet: 2 columns
- Mobile: 1 column

# 4. Component Design Standards (Very Important)

## 4.1 Button System

### Primary Button

- Background: blue
- Text: white
- Used for main actions (Buy, Add, Save)

### Secondary Button

- Border only
- Transparent background

### Danger Button

- Red background
- Used for delete/remove

Rules:

- Same padding everywhere
- Same height everywhere
- Hover effect mandatory

## 4.2 Product Card (Core UI Component)

Every product card must follow this structure:

- Image (top section)
- Product name (max 2 lines)
- Price (highlighted)
- Action button

Rules:

- No extra information clutter
- Consistent card height
- Hover effect required (slight lift or shadow)

## 4.3 Input Fields

- Full width inputs
- Border: light gray
- Focus: blue border
- Padding: 10–12px

## 4.4 Tables (Admin Panel)

- Clean row separation
- Hover highlight row
- Actions column always right aligned

## 4.5 Cards (Admin Dashboard)

Used for:

- Revenue
- Orders
- Users

Rules:

- Large number (primary focus)
- Small label below
- Icon optional

# 5. Page-Level UI Rules

## 5.1 Product Listing Page

Must include:

- Search bar (top)
- Filter section (left or top)
- Product grid

Reference behavior:

- Amazon-style layout (simplified)

## 5.2 Product Details Page

Layout:

- Left: image
- Right: details

Must include:

- Name (big)
- Price (highlighted)
- Add to cart button

## 5.3 Cart Page

Must include:

- List of items
- Quantity controls
- Price summary box
- Checkout button fixed or prominent

## 5.4 Checkout Page

Must be minimal:

- Address section
- Order summary
- Pay button

No distractions.

## 5.5 Admin Dashboard

Must include:

- Sidebar navigation
- Top stats cards
- Tables for orders/products

Reference style:

- Stripe dashboard (simplified)

# 6. CSS Modules Rules (Strict Coding Standards)

## 6.1 File Structure Rule

Every component must have:

```
ComponentName/
  index.jsx
  styles.module.css
```

## 6.2 Naming Convention

- Use camelCase in CSS modules
- Example:
  - `productCard`
  - `addButton`
  - `priceText`

## 6.3 No Global CSS Rule

- No inline styles
- No global CSS except:
  - resets
  - typography base
  - variables (optional)

## 6.4 Reusability Rule

If style is repeated 3+ times → make a reusable component.

# 7. UI Quality Checklist (Before Submission)

Every student must verify:

## Layout

- Consistent spacing everywhere
- No misaligned elements

## UX

- Every action is clear
- Buttons are visible and consistent

## Visual Consistency

- Same card design everywhere
- Same button styles everywhere

## Responsiveness

- Works on mobile and desktop

# 8. Final UI Expectation Summary

At the end, the project should visually look like:

- Clean e-commerce product grid (Amazon-like simplified)
- Professional admin dashboard (Stripe-like simplified)
- Minimal checkout flow (Stripe-inspired)
- Consistent modern spacing and typography

Not:

- Random student CSS project
- Unstructured UI
- Different styles on every page
