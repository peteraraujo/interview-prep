## Preventing Memory Leaks in React



A memory leak occurs when an application allocates memory for a process or component but fails to release that memory when it is no longer needed. In React, this typically happens when a component unmounts from the Document Object Model (DOM), yet asynchronous operations, global objects, or closures continue to reference the component's state or internal functions. The browser's garbage collector cannot free the associated memory.

Over time, accumulated memory leaks cause increased memory consumption, sluggish performance, and eventual application crashes.







### Common Causes and Prevention Strategies



Preventing memory leaks depends on properly utilizing the cleanup function returned by the `useEffect` hook. Any persistent connection or process started inside an effect must be dismantled when the component unmounts.



#### 1\. Uncleared Timers and Intervals



**The Issue:** When `setInterval` or `setTimeout` is initiated inside a component, the callback retains a closure over the component's state. If the component unmounts before the timer is cancelled, it continues executing in the background and holds onto memory.



**The Solution:** Always clear timers in the effect's cleanup function.



```jsx
import { useEffect, useState } from 'react';

const TimerComponent = () => {
  const \[count, setCount] = useState(0);

  useEffect(() => {
    const timerId = setInterval(() => {
      setCount((prev) => prev + 1);
    }, 1000);

    // Cleanup phase: clears the interval when the component unmounts
    return () => clearInterval(timerId); 
  }, \[]);

  return <div>{count}</div>;
};
```



#### 2\. Unremoved Global Event Listeners



**The Issue:** Attaching event listeners to `window` or `document` creates a persistent reference to the component's callback. When the component unmounts, the listener remains active and continues attempting to execute logic on a non-existent component.

**The Solution:** Remove the listener using the exact same function reference in the cleanup phase.

```jsx
import { useEffect, useState } from 'react';

const ScrollTracker = () => {
  const \[scrollY, setScrollY] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setScrollY(window.scrollY);
    };

    window.addEventListener('scroll', handleScroll);

    // Cleanup phase: removes the listener when the component unmounts
    return () => window.removeEventListener('scroll', handleScroll); 
  }, \[]);

  return <div>Scrolled: {scrollY}px</div>;
};
```

#### 3\. Unresolved Asynchronous Requests

**The Issue:** Network requests (such as `fetch` or Axios calls) may still be pending when a user navigates away. When the request eventually resolves, the `.then()` or `await` block attempts to update state on a destroyed component, wasting processing power and retaining memory.

**The Solution:** Use an `AbortController` to cancel pending requests on unmount.

```jsx
import { useEffect, useState } from 'react';

const DataFetcher = () => {
  const \[data, setData] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    const fetchData = async () => {
      try {
        const response = await fetch('/api/data', { signal: controller.signal });
        const result = await response.json();
        setData(result);
      } catch (error) {
        if (error.name === 'AbortError') {
          console.log('Fetch successfully aborted upon unmount');
        }
      }
    };

    fetchData();

    // Cleanup phase: aborts the fetch request if the component unmounts early
    return () => controller.abort(); 
  }, \[]);

  return <div>{data ? 'Data loaded' : 'Loading...'}</div>;
};
```

#### 4\. Active Subscriptions (WebSockets, Observables, Global Stores)

**The Issue:** Subscribing to real-time data streams, WebSockets, or external state managers creates an ongoing link between the data source and the component. Failing to unsubscribe keeps the component in memory indefinitely.

**The Solution:** Call the unsubscribe method provided by the service in the cleanup phase.

```jsx
import { useEffect, useState } from 'react';
import { someRealtimeService } from './services';

const LiveFeed = () => {
  const \[feed, setFeed] = useState(\[]);

  useEffect(() => {
    const subscription = someRealtimeService.subscribe((newData) => {
      setFeed((prevFeed) => \[...prevFeed, newData]);
    });

    // Cleanup phase: unsubscribes from the service
    return () => subscription.unsubscribe(); 
  }, \[]);

  return <div>Live events incoming...</div>;
};
```

### Summary of Prevention

Preventing memory leaks requires a systematic approach to component lifecycles. Every side effect that reaches outside the React component — whether interacting with the browser API, a network, or an external data source — must include corresponding teardown logic in the `useEffect` return statement

