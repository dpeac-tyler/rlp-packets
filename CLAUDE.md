# RLP Packets — Project Guide

## What this project is
Static HTML prototype for the RLP (Regulatory Licensing Platform) **Packets** feature. Packets are agency-admin-configured definitions that group a primary license/application type with one or more secondary (child) application types mapped to affiliation roles. No build step — open HTML files directly in a browser.

## Stack
- Static HTML files at the project root
- **USWDS** (US Web Design System) via `css/styles.css` — this is the source of truth for all base styles
- Custom overrides also live in `css/styles.css`
- Assets: `assets/` — icons, logos
- No JS framework; vanilla JS inline in each page

## Page inventory
| File | Purpose |
|---|---|
| `index.html` | Agency homepage — tile cards linking to major sections |
| `packets.html` | All Packets list table |
| `create-packet.html` | Create/Edit Packet form (the canonical, current version) |
| `applications.html` | All Applications list table |
| `license-types.html` | All License Types list table |

## Page structure — always follow this shell
```html
<div class="app-container">
  <div class="header management">          <!-- skinny welcome bar + logo/nav row -->
  <nav class="horizontalNav main-menu">   <!-- horizontal primary nav -->
  <main id="main-content" class="grid-container">
    <nav class="usa-breadcrumb">          <!-- breadcrumb when not on top-level page -->
    <div class="profile-header">
      <h1 class="profile-header__title">Page Title</h1>
    </div>
    <div class="profile-content">
      <div class="inspection-content">   <!-- constrains form width; all fields go here -->
        <!-- fields -->
      </div>
    </div>
    <div class="margin-top-4">           <!-- Save/Cancel OUTSIDE profile-content -->
      <button class="usa-button">Save</button>
      <a class="usa-button usa-button--secondary">Cancel</a>
    </div>
  </main>
</div>
```

## Form conventions

### Labels & required indicator
```html
<label class="usa-label" for="field-id">
  Field Name <span class="field-required">Required</span>
</label>
```

### Error pattern (field level)
```html
<div class="margin-bottom-4" id="field-group">
  <label class="usa-label" for="field-id">Label <span class="field-required">Required</span></label>
  <span class="usa-error-message" id="field-error" hidden>Error message</span>
  <input class="usa-input" id="field-id" type="text">
</div>
```
Add `usa-form-group--error` to the group div and `usa-input--error` to the input on validation failure.

### Top-level error alert
```html
<div id="error-alert" class="usa-alert usa-alert--error" hidden>
  <div class="usa-alert__body">
    <h3 class="usa-alert__heading">Error status</h3>
    <p class="usa-alert__text">Please correct the following errors before submitting.</p>
  </div>
</div>
```

### Select / dropdown
Use `<select class="usa-select">` for short known lists. For lists of 10+ items use the **combobox** pattern (see below).

### Combobox (type-ahead single-select)
Use when the list could be 10+ items. Pattern: text input + hidden input + `<ul>` dropdown.
```html
<div class="combo-box" id="my-combo">
  <input class="usa-input combo-box__input" type="text" id="my-field-input"
         placeholder="Please Select" autocomplete="off"
         aria-expanded="false" aria-controls="my-combo-list">
  <input type="hidden" id="my-field" name="my-field">
  <ul class="combo-box__list" id="my-combo-list" role="listbox" hidden></ul>
</div>
```
Init with `initComboBox(textInputId, hiddenInputId, listId, optionsArray)`. The hidden input holds the value; the text input is display-only. Validation checks `hiddenInput.value`.

### Checkbox (USWDS)
```html
<div class="usa-checkbox">
  <input class="usa-checkbox__input" id="field-id" type="checkbox" name="field-id">
  <label class="usa-checkbox__label" for="field-id">Label</label>
</div>
```

### Radio group (inline)
```html
<div class="type-radios"> <!-- flex row, gap 16px -->
  <label><input type="radio" name="group" value="a"> Option A</label>
  <label><input type="radio" name="group" value="b"> Option B</label>
</div>
```

## Section headings (inside forms)
```html
<h2 class="section-heading">Section Title</h2>
```
CSS: `font-size: 1.9rem; font-weight: 700; border-bottom: 2px solid #dfe1e2; padding-bottom: 8px; margin: 32px 0 16px;`

## Add-item form pattern (questions-section style)
Used when the user adds rows to a table inline. The add form sits **above** the table; the table grows below.
```html
<div class="add-secondary-form">        <!-- #f0f0f0 bg, 2rem padding, 0.5rem border-radius -->
  <div class="add-secondary-grid">     <!-- grid: 70% 15% 15%, align-items: end -->
    <div><!-- col 1: main input --></div>
    <div><!-- col 2: secondary control (radios etc.) --></div>
    <div><!-- col 3: accent-warm Add button --></div>
  </div>
  <span class="usa-error-message" id="add-form-error" hidden></span>
</div>
<!-- table renders below -->
```
The "Add" button uses `class="usa-button usa-button--accent-warm"` (orange, `#fa9441`).

## Table conventions
- Always: `class="usa-table usa-table--borderless usa-table--striped"`
- Precede with table controls (showing X of Y + entries-per-page select)
- Controls column: `<th scope="col">Controls</th>` — always last; set `white-space: nowrap; width: 120px;` on th and `white-space: nowrap;` on each td
- Empty state: `<p id="no-X-msg" class="text-base">No X added yet.</p>` shown when table is hidden

## Icon key accordion
Add above the table controls on any list page with a Controls column.
```html
<div class="usa-accordion usa-accordion--bordered">
  <h3 class="usa-accordion__heading">
    <button type="button" class="usa-accordion__button" aria-expanded="false" aria-controls="icon-key-content">
      Icon Key
    </button>
  </h3>
  <div id="icon-key-content" class="usa-accordion__content usa-prose" hidden="">
    <p class="text-bold margin-0 margin-bottom-2">Options and Control Types</p>
    <div class="icon-key-grid">
      <div class="icon-key-item">
        <img src="assets/eye-icon.png" alt="" width="24" height="24">
        <span>- View [Item]</span>
      </div>
      <!-- edit-icon.png, icon-delete.png as needed -->
    </div>
  </div>
</div>
```
Requires the accordion JS snippet (see below).

## Icons (all in `assets/`)
| File | Use | Size in tables | Size in icon key |
|---|---|---|---|
| `eye-icon.png` | View | 30×30 | 24×24 |
| `edit-icon.png` | Edit | 30×30 | 24×24 |
| `icon-delete.png` | Delete | 30×30 | 24×24 |
| `publish-icon.png` | Publish | 30×30 | 24×24 |
| `discard-icon.png` | Discard draft | 30×30 | 24×24 |
| `agency-logo.png` | Header logo | — | — |
| `rlp-logo.png` | Header nav home | height 30 | — |

Always wrap in `<a href="#" class="icon-button" aria-label="[Action] [Item]">`.

## Button classes
| Class | Color | Use |
|---|---|---|
| `usa-button` | Navy blue | Primary action (Save, Create New, etc.) |
| `usa-button--secondary` | Outline | Cancel, secondary actions |
| `usa-button--accent-warm` | Orange `#fa9441` | Add-item actions within forms |
| `usa-button--unstyled` | None | Icon-only buttons |

## Reusable JS snippets

### Accordion
```js
document.querySelectorAll('.usa-accordion__button').forEach(button => {
  button.addEventListener('click', function() {
    const expanded = this.getAttribute('aria-expanded') === 'true';
    const content = document.getElementById(this.getAttribute('aria-controls'));
    if (expanded) {
      this.setAttribute('aria-expanded', 'false');
      content.setAttribute('hidden', '');
    } else {
      this.setAttribute('aria-expanded', 'true');
      content.removeAttribute('hidden');
    }
  });
});
```

### Combobox init
```js
function initComboBox(textInputId, hiddenInputId, listId, options) {
  const textInput = document.getElementById(textInputId);
  const hiddenInput = document.getElementById(hiddenInputId);
  const list = document.getElementById(listId);
  function renderList(query) {
    const q = query.toLowerCase().trim();
    const matches = q ? options.filter(o => o.label.toLowerCase().includes(q)) : options;
    list.innerHTML = '';
    if (matches.length === 0) {
      const li = document.createElement('li');
      li.className = 'combo-box__option combo-box__option--no-results';
      li.textContent = 'No matching options';
      list.appendChild(li);
    } else {
      matches.forEach(opt => {
        const li = document.createElement('li');
        li.className = 'combo-box__option';
        li.textContent = opt.label;
        li.addEventListener('mousedown', e => {
          e.preventDefault();
          textInput.value = opt.label;
          hiddenInput.value = opt.value;
          list.setAttribute('hidden', '');
          textInput.setAttribute('aria-expanded', 'false');
        });
        list.appendChild(li);
      });
    }
    list.removeAttribute('hidden');
    textInput.setAttribute('aria-expanded', 'true');
  }
  textInput.addEventListener('input', function() { hiddenInput.value = ''; renderList(this.value); });
  textInput.addEventListener('focus', function() { renderList(this.value); });
  textInput.addEventListener('blur', function() {
    setTimeout(() => {
      list.setAttribute('hidden', ''); textInput.setAttribute('aria-expanded', 'false');
      if (!hiddenInput.value) textInput.value = '';
    }, 150);
  });
}
```

## Packets domain concepts
- **Packet**: An agency-admin-configured definition grouping a primary + one or more secondary application types
- **Primary application type**: The parent; must have an AFQ with a review step configured
- **Secondary application type**: Child types; each is Required (must complete before packet submits) or Optional (can be skipped)
- **Auto-submit**: If enabled, packet submits automatically when all required secondaries are complete; otherwise primary user must sign off
- **Packet definition stability**: In-flight packets use the definition as it existed at creation time — changes only affect new packets
- Minimum valid packet: 1 primary + at least 1 required secondary
