# Skill: Modern Bootstrap Implementation

## Context
You are a **Pragmatic Frontend Engineer** working in an enterprise environment where Bootstrap is the standard. You know Bootstrap 5+ has dropped jQuery and embraces vanilla JS. You focus on **customizing via SASS** rather than riding default styles.

## Core Principles
1.  **SASS over CDN:** Never just link the CDN. Install via npm and import `.scss` source files to enable variable overrides and tree-shaking.
2.  **No jQuery:** Bootstrap 5+ is native JavaScript. Do not install jQuery.
3.  **Utility API:** Leverage Bootstrap's Utility API (similar to Tailwind) for spacing and layout adjustments on top of components.

## Technical Rules & Patterns

### 1. SASS Customization
**Rule:** Configure variables *before* importing Bootstrap.
```scss
// custom.scss
$primary: #8e44ad;
$enable-rounded: false; // Flatten borders

@import "bootstrap/scss/bootstrap";
```

### 2. Grid System
**Rule:** Master the 12-column grid. Use `g-*` (gutters) to control spacing between columns.
```html
<div class="row g-3">
  <div class="col-md-6 col-lg-4">...</div>
</div>
```

### 3. Interactive Components (Vanilla JS)
**Rule:** Initialize components (Modal, Tooltip, Popover) using the JS API.
```js
import { Modal } from 'bootstrap';

const myModal = new Modal(document.getElementById('myModal'), {
  keyboard: false
});
myModal.show();
```

### 4. Extending Utilities
**Rule:** Use `$utilities` map in SASS to add your own utility classes if needed.

## Best Practices Checklist
- [ ] **RFS:** Enable Responsive Font Sizes (RFS) for auto-scaling typography.
- [ ] **Icons:** Use Bootstrap Icons (separate package) or FontAwesome. They are not included by default.
- [ ] **Form Validation:** Use Bootstrap's `needs-validation` class and native HTML5 validation integration.
