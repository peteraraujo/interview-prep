## Native HTML `<dialog>` in React (Modern Modal Pattern)

---

For years, building a dialog (or modal) in React was a nightmare. You had to use React Portals to append it to the `<body>`, write complex CSS to handle z-index wars, and write custom JavaScript to trap the user's keyboard focus and listen for the Escape key.

Today, the industry standard has shifted. You no longer need heavy libraries or complex portals. You should use the native HTML `<dialog>` element combined with a React `useRef`.

The browser itself now handles the backdrop, the centering, the focus trapping, and the Escape key automatically.

---

## The Implementation

Because the native `<dialog>` element relies on DOM methods (`.showModal()` and `.close()`) rather than just a CSS display property, we use the `useRef` hook to trigger it.

JavaScript

```js
import { useRef } from 'react';

export default function UserProfile() {
  // 1. Create a reference to the dialog DOM element
  const dialogRef = useRef(null);

  // 2. Helper functions to trigger the native browser methods
  const openDialog = () => dialogRef.current.showModal();
  const closeDialog = () => dialogRef.current.close();

  return (
    <div>
      <h1>User Dashboard</h1>

      {/* Button to open the dialog */}
      <button onClick={openDialog}>Delete Account</button>

      {/* 3. The Dialog Element */}
      <dialog ref={dialogRef} className="custom-dialog">
        <h2>Are you absolutely sure?</h2>
        <p>This action cannot be undone.</p>

        <div className="dialog-actions">
          {/* A form with method="dialog" automatically closes the dialog on submit */}
          <form method="dialog">
            <button>Cancel</button>
          </form>

          <button onClick={closeDialog} className="danger-btn">
            Yes, Delete
          </button>
        </div>
      </dialog>
    </div>
  );
}
```

---

## Why `.showModal()` is crucial

You can open a dialog by just passing an attribute: `<dialog open>`. Do not do this.

If you just force the `open` attribute, the dialog acts like a normal inline `<div>`. It does not dim the background, and the user can still click buttons behind it.

When you trigger `dialogRef.current.showModal()`, the browser upgrades the element to a true modal:

---

### The Top Layer
It completely ignores all z-index rules in your CSS and renders in a special "Top Layer" above the entire document.

---

### Focus Trapping
When the user presses Tab, their keyboard focus loops inside the modal. They cannot accidentally tab out and interact with the hidden webpage behind it.

---

### The `::backdrop`
It automatically generates a pseudo-element behind the modal that you can style to dim the screen.

CSS

```css
dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
}

dialog {
  border: none;
  border-radius: 8px;
  padding: 24px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.2);
}
```

---

## Pro Tip: Closing on Backdrop Click

One feature the native `<dialog>` does not do automatically is close when the user clicks the dark background outside the box.

Because the `::backdrop` is technically part of the dialog system, clicking outside the dialog fires a click event on the dialog element itself.

You can fix this by checking whether the click occurred outside the dialog’s bounding box.

JavaScript

```js
const handleOutsideClick = (e) => {
  const dialogNode = dialogRef.current;
  const rect = dialogNode.getBoundingClientRect();

  // Check if click is outside dialog bounds
  if (
    e.clientX < rect.left ||
    e.clientX > rect.right ||
    e.clientY < rect.top ||
    e.clientY > rect.bottom
  ) {
    dialogNode.close();
  }
};
```

Usage:

```html
<dialog ref={dialogRef} onClick={handleOutsideClick}>
```