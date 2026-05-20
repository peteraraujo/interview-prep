## JavaScript Event Loop

JavaScript is single-threaded and executes only one piece of code at a time on the main thread. The Event Loop enables asynchronous, non-blocking behavior (network requests, timers, DOM events) without freezing the page.

### 1. Call Stack
The Call Stack executes synchronous code using Last In, First Out (LIFO) order. Functions are pushed onto the stack when called and popped when they finish. Long-running synchronous code blocks the stack and freezes the UI.

### 2. Web APIs (Browser/Node APIs)
Asynchronous operations (setTimeout, fetch, event listeners) are offloaded to the browser’s Web APIs (implemented in C++). The main thread remains free while the browser handles the background work.

### 3. Queues (Task Queues)
When a Web API completes, its callback is placed in a queue rather than immediately running. Two queues exist:

- **Microtask Queue** (VIP line): Handles Promises (`.then()`, `.catch()`, `async/await`).
- **Macrotask Queue** (Task Queue): Handles timers (`setTimeout`, `setInterval`), DOM events, and UI rendering.

### 4. Event Loop (The Bouncer)
The Event Loop continuously checks:

- Is the Call Stack empty?
- Are tasks waiting in the queues?

If the stack is empty:
- It first empties the entire Microtask Queue.
- Then it processes one task from the Macrotask Queue.
- The cycle repeats.

### Classic Interview Example
```js
console.log('1. Synchronous');

setTimeout(() => {
  console.log('2. Timeout (Macrotask)');
}, 0);

Promise.resolve().then(() => {
  console.log('3. Promise (Microtask)');
});

console.log('4. Synchronous');
```

**Output (always in this order):**
```
1. Synchronous
4. Synchronous
3. Promise (Microtask)
2. Timeout (Macrotask)
```

Even with a 0 ms delay, the `setTimeout` callback waits in the Macrotask Queue until after all microtasks and synchronous code finish.