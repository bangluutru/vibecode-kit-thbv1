# ARIA Component Patterns

> Copy-paste ARIA patterns for common UI components. Based on WAI-ARIA Authoring Practices.

## Modal Dialog

```html
<button id="open-dialog" aria-haspopup="dialog">Open Settings</button>

<div id="dialog" role="dialog" aria-modal="true" aria-labelledby="dialog-title" hidden>
  <h2 id="dialog-title">Settings</h2>
  <div class="dialog-body">
    <!-- Dialog content -->
  </div>
  <div class="dialog-actions">
    <button id="cancel-btn">Cancel</button>
    <button id="save-btn" autofocus>Save</button>
  </div>
</div>
```

**Keyboard**: Escape closes, Tab traps inside, focus returns to trigger.

---

## Tab Panel

```html
<div role="tablist" aria-label="Account Settings">
  <button role="tab" id="tab-1" aria-selected="true" aria-controls="panel-1" tabindex="0">
    Profile
  </button>
  <button role="tab" id="tab-2" aria-selected="false" aria-controls="panel-2" tabindex="-1">
    Security
  </button>
  <button role="tab" id="tab-3" aria-selected="false" aria-controls="panel-3" tabindex="-1">
    Notifications
  </button>
</div>

<div role="tabpanel" id="panel-1" aria-labelledby="tab-1" tabindex="0">
  Profile settings content...
</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" tabindex="0" hidden>
  Security settings content...
</div>
<div role="tabpanel" id="panel-3" aria-labelledby="tab-3" tabindex="0" hidden>
  Notification settings content...
</div>
```

**Keyboard**: Arrow keys switch tabs, Tab moves to panel content.

---

## Accordion

```html
<div class="accordion">
  <h3>
    <button aria-expanded="true" aria-controls="sect-1" id="acc-1">
      <span>Section 1 Title</span>
      <span aria-hidden="true">▼</span>
    </button>
  </h3>
  <div id="sect-1" role="region" aria-labelledby="acc-1">
    <p>Section 1 content here...</p>
  </div>

  <h3>
    <button aria-expanded="false" aria-controls="sect-2" id="acc-2">
      <span>Section 2 Title</span>
      <span aria-hidden="true">▶</span>
    </button>
  </h3>
  <div id="sect-2" role="region" aria-labelledby="acc-2" hidden>
    <p>Section 2 content here...</p>
  </div>
</div>
```

**Keyboard**: Enter/Space toggles section.

---

## Dropdown Menu

```html
<div class="menu-container">
  <button aria-haspopup="true" aria-expanded="false" aria-controls="menu-list">
    Actions ▾
  </button>
  <ul id="menu-list" role="menu" hidden>
    <li role="menuitem" tabindex="-1">Edit</li>
    <li role="menuitem" tabindex="-1">Duplicate</li>
    <li role="separator"></li>
    <li role="menuitem" tabindex="-1">Delete</li>
  </ul>
</div>
```

**Keyboard**: Arrow keys navigate items, Enter selects, Escape closes.

---

## Toast / Status Message

```html
<!-- Non-urgent status (announced when idle) -->
<div role="status" aria-live="polite" aria-atomic="true" class="toast">
  ✅ Settings saved successfully
</div>

<!-- Urgent error (announced immediately) -->
<div role="alert" aria-live="assertive" aria-atomic="true" class="toast-error">
  ❌ Failed to save. Please try again.
</div>
```

---

## Form Error Summary

```html
<div role="alert" aria-labelledby="error-heading" class="error-summary">
  <h2 id="error-heading">There are 2 errors in the form</h2>
  <ul>
    <li><a href="#email">Email address is required</a></li>
    <li><a href="#password">Password must be at least 8 characters</a></li>
  </ul>
</div>

<div class="form-group">
  <label for="email">Email <span aria-hidden="true">*</span></label>
  <input id="email" type="email" aria-required="true" aria-invalid="true" 
         aria-describedby="email-error">
  <p id="email-error" class="field-error">Email address is required</p>
</div>
```

---

## Breadcrumb

```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/products/shoes" aria-current="page">Shoes</a></li>
  </ol>
</nav>
```

---

## Search with Autocomplete

```html
<div role="combobox" aria-expanded="false" aria-haspopup="listbox">
  <label for="search">Search products</label>
  <input id="search" type="search" 
         aria-autocomplete="list" 
         aria-controls="search-results"
         aria-activedescendant="">
  <ul id="search-results" role="listbox" hidden>
    <li role="option" id="result-1">Blue Sneakers</li>
    <li role="option" id="result-2">Red T-shirt</li>
  </ul>
</div>
```

**Keyboard**: Arrow keys navigate results, Enter selects, Escape closes.
