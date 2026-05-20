## Document Object Model (DOM)

The Document Object Model (DOM) is the browser’s internal tree-like representation of an HTML page. JavaScript interacts with the DOM API to read and modify the page dynamically after it has loaded.

### 1. Selecting Elements
Use CSS selectors for modern, flexible element selection.

```js
// First matching element
const header = document.querySelector('.main-title');

// All matching elements (NodeList)
const buttons = document.querySelectorAll('button');

// By exact ID (fastest)
const submitBtn = document.getElementById('submit-btn');
```

### 2. Modifying Content and Attributes
Update text, HTML, or attributes once an element is selected.

```js
const box = document.querySelector('.box');

// Safe text update (ignores HTML)
box.textContent = 'Hello World';

// HTML update (use with caution to avoid XSS)
box.innerHTML = '<strong>Hello World</strong>';

// Attribute manipulation
const link = document.querySelector('a');
link.setAttribute('href', 'https://example.com');
```

### 3. Modifying Styles and Classes
Apply inline styles or (preferably) manage classes.

```js
const message = document.querySelector('.message');

// Inline styles (camelCase properties)
message.style.backgroundColor = 'blue';
message.style.marginTop = '20px';

// Class manipulation (preferred approach)
message.classList.add('success');
message.classList.remove('hidden');
message.classList.toggle('active');
```

### 4. Creating and Appending Elements
Build new elements in memory, then attach them to the live DOM.

```js
// Create element
const newListItem = document.createElement('li');

// Configure it
newListItem.textContent = 'Buy groceries';
newListItem.classList.add('todo-item');

// Append to parent
const list = document.querySelector('#todo-list');
list.appendChild(newListItem);
```

### 5. Removing Elements
Remove elements cleanly from the DOM.

```js
const banner = document.querySelector('.promo-banner');

// Modern direct removal
banner.remove();

// Legacy parent removal
banner.parentNode.removeChild(banner);
```

### 6. Event Handling
Listen for user interactions and respond with callbacks.

```js
const button = document.querySelector('.save-btn');

button.addEventListener('click', function(event) {
  console.log('Button clicked at X:', event.clientX);
  
  // Prevent default browser behavior
  event.preventDefault();
});

