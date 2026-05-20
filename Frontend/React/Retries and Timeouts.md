## Resilient Fetching in React (Timeouts + Retries)

In React applications, networks are unreliable. A standard `fetch()` can hang indefinitely or fail silently on a flaky connection. Resilient data fetching requires two mechanisms:

- **Timeouts**: Abort a request if it takes too long.
- **Retries**: Automatically retry transient failures (with optional backoff).

### 1. Native Vanilla Approach
Use `AbortController` for timeouts and a retry loop for reliability. Here is a reusable custom hook:

```jsx
import { useState, useEffect } from 'react';

export function useFetchWithRetry(url, maxRetries = 3, timeoutMs = 3000) {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    let isMounted = true;

    const fetchData = async () => {
      setIsLoading(true);
      setError(null);

      for (let attempt = 1; attempt <= maxRetries; attempt++) {
        const controller = new AbortController();
        const timeoutId = setTimeout(() => controller.abort(), timeoutMs);

        try {
          const response = await fetch(url, { signal: controller.signal });
          clearTimeout(timeoutId);

          if (!response.ok) throw new Error(`HTTP ${response.status}`);

          const result = await response.json();
          if (isMounted) {
            setData(result);
            setIsLoading(false);
          }
          return; // Success → exit loop
        } catch (err) {
          clearTimeout(timeoutId);
          const isTimeout = err.name === 'AbortError';
          const message = isTimeout ? 'Request timed out' : err.message;

          console.warn(`Attempt ${attempt} failed: ${message}`);

          if (attempt === maxRetries) {
            if (isMounted) {
              setError(`Failed after ${maxRetries} attempts. Last error: ${message}`);
              setIsLoading(false);
            }
            return;
          }

          // Optional exponential backoff
          await new Promise((resolve) => setTimeout(resolve, 1000));
        }
      }
    };

    fetchData();

    return () => { isMounted = false; };
  }, [url, maxRetries, timeoutMs]);

  return { data, error, isLoading };
}
```

### 2. Modern Industry Standard (TanStack Query)
Manually managing `AbortController`, retries, and unmount safety is error-prone. The recommended approach is TanStack Query (formerly React Query). It provides built-in retries, exponential backoff, and automatic cancellation.

```jsx
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

const fetchUserData = async ({ signal }) => {
  const { data } = await axios.get('/api/users', {
    signal,
    timeout: 3000, // Built-in timeout
  });
  return data;
};

export default function UserProfile() {
  const { data, error, isLoading, failureCount } = useQuery({
    queryKey: ['userData'],
    queryFn: fetchUserData,
    retry: 3,                          // Auto-retry 3 times
    retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 10000), // Exponential backoff
  });

  if (isLoading) return <p>Loading... (Failed attempts: {failureCount})</p>;
  if (error) return <p>Error: {error.message}</p>;

  return <div>{data.name}</div>;
}
```

**Recommendation:** Use TanStack Query for production. It eliminates boilerplate while giving you timeouts, retries, caching, and background refetching out of the box.

