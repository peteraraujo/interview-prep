# Infinite List

Implementing infinite scrolling in React is usually done using the browser's native Intersection Observer API. It is
highly performant because it asynchronously monitors when an element (like a loading spinner or an invisible div at the
bottom of your list) enters the viewport, rather than firing hundreds of times per second like a traditional `onScroll`
event listener.

Here is a complete, step-by-step guide on how to implement it using React Hooks.

### The Core Concept

The logic revolves around tracking the last item in your list:

Render a list of items.

Attach a "ref" to the very last element in that list.

Use an IntersectionObserver to watch that specific element.

When that element intersects with the viewport (meaning the user scrolled to the bottom), trigger a function to fetch
the next page of data.

Append the new data to your state and move the "ref" to the new last element.

### The Implementation Code

Here is a clean implementation using `useState`, `useRef`, and `useCallback`.

```js
import React, {useState, useEffect, useRef, useCallback} from 'react';

// A mock function to simulate an API call
const fetchMockData = async (page) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            const newData = Array.from({length: 20}, (_, i) => `Item ${(page - 1) * 20 + i + 1}`);
            resolve(newData);
        }, 1000); // 1 second delay to simulate network latency
    });
};

export default function InfiniteScrollList() {
    const [items, setItems] = useState([]);
    const [page, setPage] = useState(1);
    const [loading, setLoading] = useState(false);
    const [hasMore, setHasMore] = useState(true);

    // 1. We need a ref to store the actual IntersectionObserver instance
    const observer = useRef();

    // 2. We use a callback ref to attach to our last element.
    // This triggers every time the last element changes (when new data loads).
    const lastElementRef = useCallback(
        (node) => {
            if (loading) return; // Don't trigger if we are already loading

            // If there's an existing observer, disconnect it from the previous last element
            if (observer.current) observer.current.disconnect();

            // Create a new observer
            observer.current = new IntersectionObserver((entries) => {
                // entries[0].isIntersecting is true when the element enters the viewport
                if (entries[0].isIntersecting && hasMore) {
                    setPage((prevPage) => prevPage + 1); // Increment page number
                }
            });

            // Tell the observer to watch the new node (the new last element)
            if (node) observer.current.observe(node);
        },
        [loading, hasMore]
    );

    // 3. Fetch data whenever the 'page' state changes
    useEffect(() => {
        const loadItems = async () => {
            setLoading(true);
            const newItems = await fetchMockData(page);

            setItems((prevItems) => [...prevItems, ...newItems]);
            setHasMore(newItems.length > 0); // Stop if the API returns an empty array
            setLoading(false);
        };

        loadItems();
    }, [page]);

    return (
        <div style={{maxWidth: '400px', margin: '0 auto', fontFamily: 'sans-serif'}}>
            <h2>Infinite Scroll List</h2>

            <div style={{border: '1px solid #ccc', borderRadius: '8px', padding: '16px'}}>
                {items.map((item, index) => {
                    // If it's the last item in the array, attach our callback ref
                    if (items.length === index + 1) {
                        return (
                            <div
                                ref={lastElementRef}
                                key={item}
                                style={{padding: '20px', borderBottom: '1px solid #eee'}}
                            >
                                {item}
                            </div>
                        );
                    } else {
                        return (
                            <div
                                key={item}
                                style={{padding: '20px', borderBottom: '1px solid #eee'}}
                            >
                                {item}
                            </div>
                        );
                    }
                })}

                {/* Loading Indicator */}
                {loading && <p style={{textAlign: 'center', padding: '20px'}}>Loading more items...</p>}

                {/* End of List Indicator */}
                {!hasMore && <p style={{textAlign: 'center', padding: '20px'}}>You've reached the end!</p>}
            </div>
        </div>
    );
}
```

### Why useCallback instead of useRef for the element?

You might wonder why we use `useCallback` for `lastElementRef` instead of a standard `useRef`.

If we used `useRef(null)` and attached it to the last div, React wouldn't automatically notify us when that div mounts
or
unmounts (which happens every time we add new items to the bottom of the list). By using a callback ref (`useCallback`),
React runs that function exactly when the element renders, allowing us to easily disconnect the old observer and attach
a new one to the freshly rendered final item.
