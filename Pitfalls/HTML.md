## HTML Pitfalls and Best Practices

---

### 1. Accidental Form Submission
HTML:
```html
<form>
  <input type="text" name="username">
  <button>Cancel</button>
  <button>Save</button>
</form>
```

Pitfall: Clicking "Cancel" submits the form and refreshes the page.
Explanation: `<button>` defaults to `type="submit"` inside `<form>`.
Fix:

```html
<button type="button">Cancel</button>
<button type="submit">Save</button>
```

---

### 2. href="#" Jump
HTML:
```html
<a href="#" onclick="openModal()">Open Modal</a>
```

Pitfall: Clicking scrolls to top of page.
Explanation: `#` is a fragment identifier pointing to top.
Fix:

```html
<button type="button" onclick="openModal()">Open Modal</button>
```

---

### 3. target="_blank" Security Flaw
HTML:
```html
<a href="https://untrusted-site.com" target="_blank">Visit Site</a>
```

Pitfall: Tabnabbing via window.opener.
Explanation: New tab can access original window object.
Fix:

```html
<a href="https://untrusted-site.com"
   target="_blank"
   rel="noopener noreferrer">
```

---

### 4. Div Button
HTML:
```html
<div class="submit-btn" onclick="submitForm()">Submit</div>
```

Pitfall: Not keyboard accessible or screen reader friendly.
Explanation: `<div>` has no semantic meaning.
Fix:

```html
<button type="button" onclick="submitForm()">Submit</button>
```

---

### 5. Missing Viewport Meta Tag
HTML:
```html
<head>
  <title>My App</title>
</head>
```

Pitfall: Mobile renders zoomed-out desktop layout.
Explanation: Default viewport assumes ~980px width.
Fix:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

---

### 6. Placeholder as Label
HTML:
```html
<input type="text" placeholder="Email Address">
```

Pitfall: Placeholder disappears on input and is not accessible.
Explanation: Placeholder is not a persistent label.
Fix:

```html
<label for="email">Email Address</label>
<input id="email" type="email">
```

---

### 7. Empty href Attribute
HTML:
```html
<a href="">Refresh Data</a>
```

Pitfall: Reloads page and resets state.
Explanation: Empty href resolves to current URL.
Fix:

```html
<button type="button">Refresh Data</button>
```

---

### 8. Missing enctype for File Uploads
HTML:
```html
<form method="POST" action="/upload">
  <input type="file" name="file">
  <button type="submit">Upload</button>
</form>
```

Pitfall: File not transmitted.
Explanation: Default encoding does not support binary data.
Fix:

```html
<form method="POST" action="/upload" enctype="multipart/form-data">
```

---

### 9. Block Elements Inside <p>
HTML:
```html
<p>
  Text
  <ul>
    <li>Item 1</li>
  </ul>
</p>
```

Pitfall: Invalid HTML; browser auto-closes `<p>`.
Explanation: Block elements cannot be inside `<p>`.
Fix:

```html
<p>Text</p>
<ul>
  <li>Item 1</li>
</ul>
```

---

### 10. Self-Closing Script Tag
HTML:
```html
<script src="app.js" />
```

Pitfall: Invalid HTML; script may not execute correctly.
Explanation: `<script>` is NOT a void element.
Fix:

```html
<script src="app.js"></script>
```

---

### 11. Invalid List Children
HTML:
```html
<ul>
  <h2>My List</h2>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```

Pitfall: Invalid DOM structure.
Explanation: Only `<li>` elements allowed inside `<ul>`.
Fix:

```html
<h2>My List</h2>
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```

---

### 12. Missing alt Attribute
HTML:
```html
<img src="graph.png">
```

Pitfall: Screen readers cannot describe image.
Explanation: `alt` is required for accessibility.
Fix:

```html
<img src="graph.png" alt="Graph showing revenue growth over time">
```

Decorative image:

```html
<img src="decor.png" alt="">
```

---

### 13. Duplicate IDs
HTML:
```html
<input id="email">
<input id="email">
```

Pitfall: Only first element is selectable.
Explanation: IDs must be unique in DOM.
Fix:

```html
<input id="email-1">
<input id="email-2">
```

---

### 14. Missing for Attribute on Labels
HTML:
```html
<label>Accept Terms</label>
<input type="checkbox" id="terms">
```

Pitfall: Label not clickable or associated.
Fix:

```html
<label for="terms">Accept Terms</label>
<input type="checkbox" id="terms">
```

---

### 15. Using <br> for Layout Spacing
HTML:
```html
<h1>Title</h1>
<br><br><br>
<p>Content</p>
```

Pitfall: Misuses structural HTML for layout.
Explanation: `<br>` is only for line breaks.
Fix: Use CSS spacing.

```css
margin-bottom: 24px;
```

---

### 16. disabled vs readonly
HTML:
```html
<input type="text" value="123" disabled>
```

Pitfall: Value not submitted.
Explanation: Disabled inputs are excluded from submission.
Fix:

```html
<input type="text" value="123" readonly>
```

---

### 17. Missing Form Element
HTML:
```html
<input type="text">
<button onclick="login()">Login</button>
```

Pitfall: No Enter key submission.
Explanation: Forms provide native submission behavior.
Fix:

```html
<form onsubmit="login(event)">
  <input type="text">
  <button type="submit">Login</button>
</form>
```

---

### 18. Skipping Heading Levels
HTML:
```html
<h1>Main</h1>
<h3>Section</h3>
```

Pitfall: Broken accessibility outline.
Explanation: Headings define document hierarchy.
Fix:

```html
<h1>Main</h1>
<h2>Section</h2>
<h3>Subsection</h3>
```

---

### 19. Missing tbody
HTML:
```html
<table>
  <tr><td>Row 1</td></tr>
</table>
```

Pitfall: Browser inserts implicit `<tbody>`.
Explanation: DOM differs from written HTML.
Fix:

```html
<table>
  <tbody>
    <tr><td>Row 1</td></tr>
  </tbody>
</table>
```

---

### 20. Missing iframe title
HTML:
```html
<iframe src="video"></iframe>
```

Pitfall: Screen readers announce generic frame.
Explanation: iframe requires descriptive label.
Fix:

```html
<iframe src="video" title="Product demo video"></iframe>
```

---

### 21. b vs strong
HTML:
```html
<p>I am <b>very</b> angry.</p>
```

Pitfall: No semantic meaning.
Explanation: `<b>` is visual only.
Fix:

```html
<p>I am <strong>very</strong> angry.</p>
```

---

### 22. Unescaped HTML Characters
HTML:
```html
<p>x < 10 and y > 5</p>
```

Pitfall: Broken parsing.
Explanation: `<` and `>` are reserved characters.
Fix:

```html
<p>x &lt; 10 and y &gt; 5</p>
```

---

### 23. picture Without img
HTML:
```html
<picture>
  <source srcset="large.jpg">
</picture>
```

Pitfall: Nothing renders.
Explanation: `<picture>` requires `<img>` fallback.
Fix:

```html
<picture>
  <source srcset="large.jpg">
  <img src="fallback.jpg" alt="">
</picture>
```

---

### 24. Missing lang Attribute
HTML:
```html
<html>
```

Pitfall: Screen readers use wrong language rules.
Fix:

```html
<html lang="en">
```

---

### 25. Missing name Attribute
HTML:
```html
<input type="text" id="city">
```

Pitfall: Value not submitted.
Explanation: Only `name` is included in form payload.
Fix:

```html
<input type="text" id="city" name="city">
```

---

### 26. Using blockquote for layout
HTML:
```html
<blockquote>Indented text</blockquote>
```

Pitfall: Misleading semantic meaning.
Explanation: blockquote implies citation.
Fix: Use CSS.

```css
margin-left: 40px;
```

---

### 27. tabindex Greater Than 0
HTML:
```html
<input tabindex="2">
<input tabindex="1">
```

Pitfall: Breaks natural tab order.
Fix:

```html
<input tabindex="0">
<input tabindex="-1">
```

---

### 28. Nested Interactive Elements
HTML:
```html
<a href="/profile">
  <button>Follow</button>
</a>
```

Pitfall: Conflicting click behavior.
Fix:

```html
<a href="/profile">Profile</a>
<button type="button">Follow</button>
```

---

### 29. Missing autocomplete Attribute
HTML:
```html
<input name="first_name">
```

Pitfall: Poor autofill experience.
Fix:

```html
<input name="first_name" autocomplete="given-name">
```

---

### 30. Render-Blocking Script Tags
HTML:
```html
<head>
  <script src="heavy.js"></script>
</head>
```

Pitfall: Blocks rendering.
Explanation: scripts block HTML parsing.
Fix:

```html
<script src="heavy.js" defer></script>
```