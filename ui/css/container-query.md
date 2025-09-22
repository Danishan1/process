## 1. Why Container Queries?

Media queries:

```css
@media (min-width: 768px) {
  .card { font-size: 1.2rem; }
}
```

→ This works only if the **viewport** is ≥ 768px.

But what if you’re making reusable components (`.card`, `.widget`, `.sidebar`) that may be small in one place but large in another?
Media queries fail because the component doesn’t know how wide *itself* is — only the window.

**Container queries fix this**. They let components style themselves based on the size of their container, not the viewport.


## 2. Opting into Container Queries

A container must **declare itself as a container**.

```css
.card {
  container-type: inline-size; /* measure by width */
  container-name: card-container; /* optional, for naming */
}
```

* `container-type: inline-size` → lets queries react to container **width**.
* `container-type: size` → react to both **width + height** (less common).
* `container-name` → lets you target specific containers (good for nested layouts).


## 3. Writing Container Queries

Syntax is similar to media queries, but with `@container`.

```css
@container (min-width: 400px) {
  .card-content {
    font-size: 1.2rem;
    background: lightblue;
  }
}
```

This means:

> “If the nearest ancestor with `container-type` has width ≥ 400px, apply these styles.”

If you use `container-name`:

```css
@container card-container (min-width: 400px) {
  .card-content {
    font-size: 1.2rem;
  }
}
```

## 4. Example

```html
<div class="sidebar">
  <div class="card">
    <div class="card-content">Hello World</div>
  </div>
</div>

<div class="main">
  <div class="card">
    <div class="card-content">Hello Again</div>
  </div>
</div>
```

```css
/* Make .card a container */
.card {
  border: 1px solid gray;
  padding: 1rem;
  container-type: inline-size;
}

/* Style based on card’s width */
@container (max-width: 300px) {
  .card-content {
    font-size: 0.8rem;
    background: pink;
  }
}

@container (min-width: 301px) and (max-width: 600px) {
  .card-content {
    font-size: 1rem;
    background: lightblue;
  }
}

@container (min-width: 601px) {
  .card-content {
    font-size: 1.4rem;
    background: lightgreen;
  }
}
```

Now:

* If `.sidebar` makes the card narrow → small styles apply.
* If `.main` gives the card more room → larger styles apply.
* No viewport media queries needed.


## 5. Nesting and Multiple Containers

You can have multiple container contexts:

```css
@container sidebar (min-width: 200px) { ... }
@container card-container (min-width: 400px) { ... }
```

Each element looks for the **closest ancestor** with `container-type` set.

## 6. Browser Support

* Chrome 105+, Edge 105+, Safari 16+, Firefox 113+.
* Well supported in 2025 (safe to use).
* For old browsers → fallback with media queries or JS (`ResizeObserver`).


## 7. Best Practices

* Use container queries inside **component CSS**, not global CSS.
* Always set `container-type: inline-size` on the parent you want to measure.
* If you want to use container queries everywhere, make components **self-contained**.
