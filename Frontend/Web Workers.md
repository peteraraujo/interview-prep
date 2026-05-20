## Web Workers in JavaScript

JavaScript is single-threaded, so heavy tasks (large JSON parsing, image processing, complex calculations) block the main thread, freezing the UI. **Web Workers** solve this by running JavaScript in separate background OS threads.

**Golden Rule:** Workers have no access to the DOM (`document`, `window`, CSS, etc.). They can only perform computation and communicate with the main thread via messaging.

### Execution Environment
A Worker runs in a completely isolated JavaScript engine.  
**Allowed:** `fetch`, WebSockets, IndexedDB, `setTimeout`/`setInterval`, `navigator`.  
**Forbidden:** Anything that touches the page UI.

### Core Mechanics: Message Passing
Communication happens exclusively through `postMessage()` and the `message` event.

**Main Thread (`main.js`):**
```js
const myWorker = new Worker('worker.js');

myWorker.addEventListener('message', (event) => {
  console.log('Result from background:', event.data);
});

myWorker.postMessage({ command: 'process', data: [1, 2, 3, 4, 5] });
```

**Worker Thread (`worker.js`):**
```js
self.addEventListener('message', (event) => {
  const { command, data } = event.data;
  
  if (command === 'process') {
    const result = data.reduce((a, b) => a + b, 0); // Heavy work here
    self.postMessage(result);
  }
});
```

### How Workers Handle Memory
Browsers enforce a strict memory barrier. Three transfer methods exist:

1. **Structured Cloning (Default)**  
   `postMessage(data)` creates a deep copy using the Structured Clone Algorithm.  
   - **Pros:** Completely thread-safe.  
   - **Cons:** Copies the entire object → high CPU and memory cost for large data.

2. **Transferable Objects (Zero-Copy)**  
   Use for massive data (`ArrayBuffer`, `MessagePort`). The memory block is moved, not copied.  
   ```js
   const buffer = new ArrayBuffer(50 * 1024 * 1024); // 50 MB
   myWorker.postMessage(buffer, [buffer]); // Transfer ownership
   ```
   - **Pros:** Extremely fast.  
   - **Cons:** Main thread loses access permanently unless transferred back.

3. **SharedArrayBuffer (True Shared Memory)**  
   Both threads read/write the exact same RAM block.  
   - **Pros:** No copying, real-time sharing.  
   - **Cons:** Requires the `Atomics` API for safe concurrent access to prevent data corruption.

### Architecture & Best Practices
- **Startup Overhead:** Creating a Worker is expensive. Never create/destroy Workers for tiny tasks.
- **Worker Pool (Production Pattern):**  
  On app load, spawn a fixed number of Workers (use `navigator.hardwareConcurrency` to match CPU cores).  
  Keep them alive and assign tasks to idle Workers via a queue. This maximizes CPU usage without exhausting memory.

Web Workers turn the browser into a true multi-threaded environment while keeping the main UI thread responsive.

