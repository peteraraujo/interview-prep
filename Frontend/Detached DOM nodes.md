## Detached DOM Nodes

A **detached DOM node** is an HTML element that has been removed from the active Document Object Model (DOM) tree but cannot be garbage-collected because a JavaScript variable (or closure) still holds a reference to it.

This is one of the most common and dangerous memory leaks in Single Page Applications (React, Angular, Vue). DOM nodes are heavy objects; retaining a single parent node can keep thousands of child elements (tables, lists, charts) alive in memory.

### The Analogy: The Severed Branch
Think of the webpage as a living tree (the DOM):
- Removing a branch from the tree lets it fall, die, and decompose (the Garbage Collector reclaims the memory).
- If you cut the branch but immediately place it in a jar of water on your desk (a JavaScript variable), the branch stays alive even though it is no longer part of the tree.

### How It Happens in Code
The leak occurs when an element is queried into a JavaScript variable and later removed from the DOM without clearing the reference.

```js
// 1. Store a reference to a DOM element
let myButton = document.getElementById('submit-btn');

function removeButton() {
  // 2. Remove it from the live DOM tree
  document.body.removeChild(myButton);
  
  // THE LEAK: myButton still points to the node.
  // The Garbage Collector cannot free it → Detached DOM Node.
}
```

### The Fix
After removing the node from the DOM, explicitly clear all JavaScript references so the object becomes unreachable.

```js
function safelyRemoveButton() {
  document.body.removeChild(myButton);
  
  // Sever the reference so the Garbage Collector can reclaim the memory
  myButton = null;
}
```

**Note for Modern Frameworks:**  
In React (and similar libraries), you rarely use `document.getElementById`. Detached nodes still occur when you store a React `ref`, a DOM reference in global state, a closure, or a third-party library instance (e.g., a charting library) and forget to clean it up on component unmount.

