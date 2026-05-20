## Closures in JavaScript

A closure is a feature where an inner function retains access to variables from its outer (enclosing) scope, even after the outer function has finished executing.

### The Backpack Analogy
Every function is created with an invisible “backpack.” JavaScript automatically stores copies of all variables from the surrounding scope that the inner function might need later. When the inner function runs — even in a completely different part of the code — it simply opens its backpack and uses those preserved variables.

### How a Standard Closure Works
The classic counter example demonstrates the concept:

```js
function makeCounter() {
  let count = 0; // Variable lives in the outer scope

  return function increment() {
    count++;
    console.log(count);
  };
}

const myCounter = makeCounter();

myCounter(); // Logs: 1
myCounter(); // Logs: 2
```

Even though `makeCounter` has completed execution, the `increment` function still has access to `count`. This variable is preserved in a hidden **Closure Scope** on the Heap, preventing the Garbage Collector from reclaiming it.

### What Is a Retained Closure?
A retained closure occurs when a closure is kept alive in memory longer than intended — usually accidentally. Because the inner function holds references to outer variables, the entire outer scope cannot be garbage-collected. This is one of the most common causes of memory leaks in frontend applications.

If the outer scope contains large data structures (for example, a massive array), that data remains in memory for as long as the inner function exists anywhere in the application.

### Common Causes of Retained Closures

1. **Uncleared Intervals or Timers**  
   A callback inside `setInterval` (or `setTimeout`) forms a closure around outer variables. If the interval is never cleared, those variables remain trapped forever.

   ```js
   function startTracking(hugeData) {
     setInterval(() => {
       console.log('Tracking data...', hugeData.length);
     }, 1000);
   }
   // Without clearInterval, hugeData can never be garbage-collected.
   ```

2. **Event Listeners on Persistent Elements**  
   Attaching a listener to `window`, `document`, or a long-lived DOM node with a callback that references local variables creates a retained closure until the listener is explicitly removed.

   ```js
   function setupAnalytics() {
     const userHistory = new Array(1000000).fill('data'); // Heavy data

     const handleScroll = () => {
       console.log('Scrolled!', userHistory[0]);
     };

     window.addEventListener('scroll', handleScroll);
     // Must call removeEventListener on unmount to release the closure.
   }
   ```

3. **Caching or Global Storage of Functions**  
   Storing inner functions in a global array, Redux store, or React Context keeps the entire closure (and all trapped variables) alive indefinitely.

