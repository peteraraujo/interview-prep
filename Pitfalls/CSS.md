## Classic CSS Pitfalls

Here are 15 classic CSS pitfalls that frequently appear in technical interviews. They test your understanding of how the browser’s rendering engine actually calculates layout.

### 1. Margin Collapsing
```css
.box-one { margin-bottom: 20px; }
.box-two { margin-top: 30px; }
```
**Pitfall:** Expecting a 50px gap between the boxes.  
**Explanation:** Vertical margins on adjacent sibling elements collapse into a single margin. The browser takes the larger value (30px).  
**Fix:** Use `display: flex` or `display: grid` with the `gap` property instead of relying on margins.

### 2. The z-index Illusion (Stacking Contexts)
```css
.modal { z-index: 999999; }
```
**Pitfall:** The modal still appears behind a header with `z-index: 10`.  
**Explanation:** `z-index` only works within its own stacking context. A parent with `position: relative; z-index: 1` traps the modal inside level 1.  
**Fix:** Move the modal to the end of `<body>` (or use React Portals) so it lives in the root stacking context.

### 3. The Padding Blowout
```css
.input-field {
  width: 100%;
  padding: 20px;
  border: 2px solid black;
}
```
**Pitfall:** The input overflows its container and causes horizontal scrolling.  
**Explanation:** Default `box-sizing: content-box` adds padding and borders *outside* the declared width.  
**Fix:** Use `box-sizing: border-box` (apply globally with a `*` reset).

### 4. Inline Element Vertical Spacing
```css
span {
  margin-top: 50px;
  padding-bottom: 30px;
}
```
**Pitfall:** The text doesn’t move and surrounding elements are unaffected.  
**Explanation:** `display: inline` elements ignore vertical margins and padding for layout purposes.  
**Fix:** Change to `display: inline-block` (if it needs to sit inline) or `display: block`.

### 5. position: absolute Escaping
```css
.parent { /* no position */ }
.child {
  position: absolute;
  top: 0;
  left: 0;
}
```
**Pitfall:** The child jumps to the top-left corner of the entire viewport.  
**Explanation:** `position: absolute` anchors to the nearest positioned ancestor. Without one, it climbs to `<html>`.  
**Fix:** Add `position: relative` to the parent.

### 6. Percentage Padding/Margins
```css
.responsive-box {
  width: 500px;
  padding-top: 50%;
}
```
**Pitfall:** Expecting padding based on height.  
**Explanation:** Percentage padding/margin is calculated from the **width** of the containing block (so 50% = 250px).  
**Fix:** Use `aspect-ratio: 1 / 1` or `height: 100dvh` / viewport units for height-relative spacing.

### 7. The Flexbox Blowout
```css
.flex-container { display: flex; }
.flex-item { /* contains unbreakable content */ }
```
**Pitfall:** The item refuses to shrink and stretches the container.  
**Explanation:** Flex items have an implicit `min-width: auto`.  
**Fix:** Add `min-width: 0` (or `overflow: hidden`) to the flex item.

### 8. The 100vh Mobile Bug
```css
.hero-section { height: 100vh; }
```
**Pitfall:** On mobile, the bottom is hidden behind the browser UI.  
**Explanation:** Mobile browsers calculate `100vh` including the address bar.  
**Fix:** Use `height: 100dvh` (dynamic viewport height).

### 9. Em Compounding
```css
.card { font-size: 1.2em; }
```
**Pitfall:** Nested cards make text explode in size.  
**Explanation:** `em` is relative to the parent’s font size, causing compounding.  
**Fix:** Use `rem` (root em) for typography so everything stays relative to `<html>`.

### 10. Specificity Wars and !important
```css
#app-header .nav-link { color: black; }
.nav-link.active { color: blue; }
```
**Pitfall:** The active class is ignored.  
**Explanation:** Specificity points: ID (100) + class (10) beats class + class (20).  
**Fix:** Avoid IDs in CSS. Keep specificity flat (use BEM or utility classes) instead of resorting to `!important`.

### 11. transform Breaking position: fixed
```css
.animated-parent { transform: translateY(10px); }
.fixed-child { position: fixed; }
```
**Pitfall:** The fixed child scrolls with the page.  
**Explanation:** Any `transform`, `filter`, or `perspective` creates a new containing block for fixed descendants.  
**Fix:** Move the fixed element outside the transformed parent in the HTML.

### 12. Sticky Hover States on Mobile
```css
button:hover { background-color: blue; }
```
**Pitfall:** On touch devices the hover state sticks after a tap.  
**Explanation:** Touchscreens emulate hover on first tap.  
**Fix:** Wrap hover styles in `@media (hover: hover) { ... }`.

### 13. CSS Grid 1fr Blowout
```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```
**Pitfall:** One column with wide content takes more than 50%.  
**Explanation:** `1fr` is actually `minmax(auto, 1fr)`.  
**Fix:** Use `minmax(0, 1fr)`.

### 14. overflow: hidden Clipping Shadows
```css
.card {
  overflow: hidden;
  border-radius: 10px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.5);
}
```
**Pitfall:** The drop shadow disappears.  
**Explanation:** `overflow: hidden` clips anything outside the border-box, including shadows.  
**Fix:** Apply the shadow to a wrapper and `overflow: hidden` to the inner content container.

### 15. Form Element Font Inheritance
```css
body { font-family: 'Open Sans', sans-serif; }
```
**Pitfall:** `<input>`, `<button>`, and `<textarea>` still use the system font.  
**Explanation:** Form controls have their own user-agent stylesheet that overrides inheritance.  
**Fix:** Explicitly reset:
```css
input, button, textarea, select {
  font-family: inherit;
}
```

### 16. The height: 100% Failure
```css
.container { /* Parent has no explicit height */ }
.child-box { height: 100%; }
```
**Pitfall:** The child box collapses to the height of its content (or 0px if empty).  
**Explanation:** Percentage heights are calculated against a concrete parent height. If the parent uses the default `height: auto`, the browser has no base value to compute from and falls back to `auto`.  
**Fix:** Create an unbroken chain of explicit heights (`html, body { height: 100%; }`) or use `height: 100vh` / `flex-grow: 1` in a flex container.

### 17. Trapped position: sticky
```css
.main-layout { overflow-x: hidden; }
.sidebar-header { position: sticky; top: 0; }
```
**Pitfall:** The sticky header scrolls off the screen.  
**Explanation:** `position: sticky` looks for the nearest scrolling ancestor. Any parent with `overflow: hidden`, `auto`, or `scroll` (even on one axis) hijacks the sticky behavior.  
**Fix:** Ensure no ancestor has an `overflow` value, or move the sticky element outside the overflowing container.

### 18. The text-overflow: ellipsis Mirage
```css
.truncate-text { text-overflow: ellipsis; }
```
**Pitfall:** Text wraps or bleeds instead of showing "...".  
**Explanation:** `text-overflow` does nothing alone. It requires three properties at once: `white-space: nowrap`, `overflow: hidden`, and a fixed width.  
**Fix:** Always use the full trifecta:
```css
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

### 19. The opacity Inheritance Trap
```css
.modal-overlay {
  background-color: black;
  opacity: 0.5;
}
.modal-text { opacity: 1; }
```
**Pitfall:** Child text is still semi-transparent.  
**Explanation:** `opacity` is applied to the entire element and its rendering subtree as a single image. Child opacity is multiplied against the parent.  
**Fix:** Use `rgba()` for the background instead of `opacity`:
```css
background-color: rgba(0, 0, 0, 0.5);
```

### 20. Invisible Pseudo-Elements
```css
.button::before {
  display: block;
  width: 20px;
  height: 20px;
  background-color: red;
}
```
**Pitfall:** The pseudo-element never appears.  
**Explanation:** `::before` and `::after` are not generated unless they have `content`.  
**Fix:** Always declare `content: "";` (even if empty).

### 21. Layout Shift from Borders vs Outlines
```css
.card:hover { border: 2px solid blue; }
```
**Pitfall:** Hover causes the entire layout to shift.  
**Explanation:** Borders occupy physical space in the box model.  
**Fix:** Use `outline` (does not affect layout) or set a transparent border by default and only change its color on hover.

### 22. The line-height Unit Disaster
```css
.parent { font-size: 20px; line-height: 1.5em; }
.child { font-size: 10px; }
```
**Pitfall:** Child text lines overlap or look terrible.  
**Explanation:** Unit-based `line-height` is calculated once on the parent and inherited as a fixed pixel value.  
**Fix:** Use unitless `line-height: 1.5;` so the browser recalculates per child.

### 23. Viewport Width (100vw) Creating Scrollbars
```css
.full-width-banner { width: 100vw; }
```
**Pitfall:** Horizontal scrollbar appears.  
**Explanation:** `100vw` includes the space under the vertical scrollbar on Windows/Linux.  
**Fix:** Use `width: 100%` or `width: 100dvw` (dynamic viewport width).

### 24. Flexbox Image Distortion
```css
.flex-row { display: flex; }
img { width: 100px; }
```
**Pitfall:** Images stretch vertically and lose aspect ratio.  
**Explanation:** Default `align-items: stretch` forces children to match the tallest sibling.  
**Fix:** `align-self: flex-start` (or `center`) on the image.

### 25. The vertical-align Confusion
```css
.header-container { height: 100px; }
h1 { vertical-align: middle; }
```
**Pitfall:** Text stays at the top.  
**Explanation:** `vertical-align` only works on inline/inline-block elements or table cells; it aligns relative to text baseline, not the parent container.  
**Fix:** Use Flexbox: `display: flex; align-items: center;`.

### 26. CSS Selectors Can't Look Backwards
```css
/* Trying to style label based on input before it */
input:checked < label { font-weight: bold; }
```
**Pitfall:** Rule is ignored.  
**Explanation:** CSS has no parent or previous-sibling selector for performance reasons.  
**Fix:** Restructure HTML or use the modern `:has()` pseudo-class: `label:has(+ input:checked)`.

### 27. Background Bleed behind Border-Radius
```css
.badge {
  border-radius: 50%;
  border: 5px solid black;
  background-color: blue;
}
```
**Pitfall:** Blue pixels bleed through the edges of the circular border.  
**Explanation:** Default `background-clip: border-box` lets the background extend under the border.  
**Fix:** `background-clip: padding-box;`.

### 28. Mobile Safari background-attachment: fixed Jank
```css
.parallax-section {
  background-image: url('hero.jpg');
  background-attachment: fixed;
}
```
**Pitfall:** On iOS the image zooms massively or causes scrolling lag.  
**Explanation:** Mobile Safari has poor support for fixed backgrounds and recalculates against the full document height.  
**Fix:** Disable `background-attachment: fixed` on mobile or recreate the effect with a fixed pseudo-element.

### 29. Table Layout Resizing Chaos
```css
table { width: 100%; }
.col-small { width: 10%; }
```
**Pitfall:** Long unbreakable text in a narrow column ignores your width.  
**Explanation:** Default `table-layout: auto` lets content override your CSS.  
**Fix:** `table-layout: fixed;`.

### 30. The OS appearance Override
```css
select {
  background-color: darkblue;
  color: white;
  border-radius: 8px;
}
```
**Pitfall:** On iOS the dropdown still looks like the native system control.  
**Explanation:** Form controls are rendered by the OS, not the browser.  
**Fix:** Strip OS styling with `appearance: none; -webkit-appearance: none;`.

