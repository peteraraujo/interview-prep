# CSS Pseudo-Classes and Pseudo-Elements Reference

Here is a clean, interview-ready reference for the most important pseudo-classes and pseudo-elements. Organized by category for quick recall.

## Pseudo-Classes
### State / Interaction

- **`:hover`** — Element being hovered.  
  ```css
  button:hover {
    background: black;
  }
  ```

- **`:focus`** — Element currently focused (mouse or keyboard).  
  ```css
  input:focus {
    outline: 2px solid blue;
  }
  ```

- **`:focus-visible`** — Shows focus ring only for keyboard navigation (prevents ugly rings on mouse clicks).  
  ```css
  button:focus-visible {
    outline: 2px solid orange;
  }
  ```

- **`:active`** — While being clicked/tapped.  
  ```css
  button:active {
    transform: scale(0.98);
  }
  ```

- **`:disabled`** — Disabled form controls.  
  ```css
  button:disabled {
    opacity: 0.5;
  }
  ```

- **`:checked`** — Checked checkbox or radio button.  
  ```css
  input:checked + label {
    font-weight: bold;
  }
  ```

### Structural

- **`:first-child`** — First sibling.  
  ```css
  li:first-child {
    color: red;
  }
  ```

- **`:last-child`** — Last sibling.  
  ```css
  li:last-child {
    border: none;
  }
  ```

- **`:nth-child()`** — Target by position (e.g., even rows).  
  ```css
  tr:nth-child(even) {
    background: #eee;
  }
  ```

- **`:not()`** — Exclude matches.  
  ```css
  button:not(.primary) {
    opacity: 0.7;
  }
  ```

- **`:is()`** — Group selectors cleanly (adds specificity).  
  ```css
  :is(h1, h2, h3) {
    font-family: sans-serif;
  }
  ```

- **`:where()`** — Group selectors with zero specificity (great for resets).  
  ```css
  :where(nav a) {
    text-decoration: none;
  }
  ```

- **`:has()`** — Parent/conditional selector (very powerful).  
  ```css
  .card:has(img) {
    padding-top: 0;
  }
  form:has(input:invalid) {
    border: 1px solid red;
  }
  ```

### Form / Validation

- **`:required`** — Required inputs.  
  ```css
  input:required {
    border-left: 3px solid orange;
  }
  ```

- **`:valid`** — Valid form value.  
  ```css
  input:valid {
    border-color: green;
  }
  ```

- **`:invalid`** — Invalid form value.  
  ```css
  input:invalid {
    border-color: red;
  }
  ```

- **`:placeholder-shown`** — Input currently showing placeholder text.  
  ```css
  input:placeholder-shown {
    background: #fafafa;
  }
  ```

### Document / Misc

- **`:root`** — Document root (`<html>`). Perfect for CSS variables.  
  ```css
  :root {
    --primary: blue;
  }
  ```

- **`:empty`** — Element with no children or text.  
  ```css
  div:empty {
    display: none;
  }
  ```

- **`:target`** — Element targeted by URL hash.  
  ```css
  #pricing:target {
    background: yellow;
  }
  ```

## Pseudo-Elements
### Content / Text

- **`::before`** — Insert content before the element.  
  ```css
  button::before {
    content: "→ ";
  }
  ```

- **`::after`** — Insert content after the element.  
  ```css
  .external::after {
    content: " ";
  }
  ```

- **`::first-letter`** — Style the first letter.  
  ```css
  p::first-letter {
    font-size: 2rem;
  }
  ```

- **`::first-line`** — Style the first line only.  
  ```css
  p::first-line {
    font-weight: bold;
  }
  ```

- **`::selection`** — Style highlighted text.  
  ```css
  ::selection {
    background: yellow;
  }
  ```

- **`::placeholder`** — Style input placeholder text.  
  ```css
  input::placeholder {
    color: gray;
  }
  ```

### Lists / Markers

- **`::marker`** — Style list bullets or numbers.  
  ```css
  li::marker {
    color: red;
  }
  ```

### File Inputs

- **`::file-selector-button`** — Style the file upload button.  
  ```css
  input::file-selector-button {
    padding: 4px 8px;
  }
  ```

### Dialog / Backdrop

- **`::backdrop`** — Style the backdrop of a `<dialog>` or fullscreen element.  
  ```css
  dialog::backdrop {
    background: rgba(0,0,0,0.5);
  }
  ```

### Details / Summary

- **`::details-content`** — Targets the expandable content of `<details>`.  
  (Limited browser support)

### Scrollbars (WebKit-based)

- **`::-webkit-scrollbar`** — Entire scrollbar.  
- **`::-webkit-scrollbar-thumb`** — Draggable handle.  
- **`::-webkit-scrollbar-track`** — Scrollbar track.

### Media / Special

- **`::cue`** — Style video subtitles/captions.  
- **`::part()`** — Style exposed Shadow DOM parts (Web Components).  
  ```css
  my-element::part(button) {
    color: red;
  }
  ```

- **`::slotted()`** — Style slotted content inside Shadow DOM (Web Components only).

