## JavaScript Workers

JavaScript is single-threaded, so long-running tasks on the main thread freeze the entire UI (no clicks, scrolling, or animations). **Workers** solve this by running code on separate background threads.

**Golden Rule:** Workers cannot access the DOM. They communicate with the main thread exclusively through `postMessage()` and the `message` event.

### 1. Web Workers (Dedicated Workers)
Used for heavy computation (large JSON parsing, image processing, complex math).

**Main Thread (`main.js`):**
```js
// Create worker
const myWorker = new Worker('worker.js');

// Listen for messages from worker
myWorker.addEventListener('message', (event) => {
  console.log('Worker says:', event.data);
});

// Send data to worker
myWorker.postMessage([1000000000, 500000000]);
```

**Worker Thread (`worker.js`):**
```js
self.addEventListener('message', (event) => {
  const [num1, num2] = event.data;
  
  // Heavy work happens here (off main thread)
  const result = num1 * num2;
  
  // Send result back
  self.postMessage(result);
});
```

### 2. Service Workers
Act as a proxy between your app and the network. They intercept requests, cache responses, and enable offline functionality (core of Progressive Web Apps).

**Main Thread (`main.js`):**
```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
    .then(() => console.log('Service Worker Registered!'))
    .catch(err => console.error('Registration failed:', err));
}
```

**Service Worker Thread (`sw.js`):**
```js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((cachedResponse) => {
      // Return cached version if available (works offline)
      // Otherwise fetch from network
      return cachedResponse || fetch(event.request);
    })
  );
});
```

