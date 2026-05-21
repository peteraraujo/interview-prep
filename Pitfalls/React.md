## React Pitfalls

---

### 1. Direct State Mutation
JavaScript

```js
const [user, setUser] = useState({ name: 'Alice', age: 20 });

function haveBirthday() {
  user.age = 21;
  setUser(user);
}
```

The Pitfall: The user's age updates in memory, but the component never re-renders to show the new age on screen.
The Explanation: React relies on referential equality (oldState === newState) to know when to re-render. Because you mutated the existing object, the memory pointer hasn't changed. React sees the same object reference and assumes no update is needed.
The Fix: Always pass a brand new object or array.

```js
setUser({ ...user, age: 21 });
```

---

### 2. The useEffect Infinite Loop
JavaScript

```js
const [data, setData] = useState([]);

useEffect(() => {
  fetchData().then(res => setData(res));
});
```

The Pitfall: The browser tab freezes and your backend is hammered with thousands of network requests per second.
The Explanation: If you omit the dependency array entirely, useEffect runs after every single render. The effect fetches data and calls setData. setData triggers a re-render. The re-render triggers the effect again.
The Fix: Add an empty dependency array to run it only once on mount.

```js
useEffect(() => {
  fetchData().then(res => setData(res));
}, []);
```

---

### 3. Stale State Closures
JavaScript

```js
const [count, setCount] = useState(0);

useEffect(() => {
  const timer = setInterval(() => {
    setCount(count + 1);
  }, 1000);

  return () => clearInterval(timer);
}, []);
```

The Pitfall: The counter goes up to 1 and stays there forever.
The Explanation: The empty dependency array [] locks the useEffect closure on the very first render, when count was 0. Every second, the interval runs setCount(0 + 1). It never sees the updated count.
The Fix: Use functional state update.

```js
setCount(prev => prev + 1);
```

---

### 4. Object/Array Dependency Churn
JavaScript

```js
function Profile({ userId }) {
  const options = { id: userId, format: 'json' };

  useEffect(() => {
    fetchProfile(options);
  }, [options]);
}
```

The Pitfall: The useEffect triggers continuously.
The Explanation: A new object is created every render, so dependency reference changes every time.
The Fix: Remove object from deps or memoize.

```js
useEffect(() => {
  fetchProfile({ id: userId, format: 'json' });
}, [userId]);
```

---

### 5. Using Index as a Key
JavaScript

```js
{items.map((item, index) => (
  <ListItem key={index} data={item} />
))}
```

The Pitfall: UI state gets mixed up when list changes.
The Explanation: React uses keys for identity. Index changes when items shift.
The Fix: Use stable IDs.

```js
key={item.id}
```

---

### 6. Conditional Hooks
JavaScript

```js
function UserProfile({ userId }) {
  if (!userId) return <p>Loading...</p>;

  const [data, setData] = useState(null);
}
```

The Pitfall: “Rendered more hooks than during previous render” error.
The Explanation: Hooks must always execute in the same order.
The Fix: Move hooks above conditionals.

```js
const [data, setData] = useState(null);

if (!userId) return <p>Loading...</p>;
```

---

### 7. Deriving State in useEffect
JavaScript

```js
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState('');

useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);
```

The Pitfall: Extra render cycle for derived value.
The Explanation: Derived state is stored unnecessarily.
The Fix: Compute during render.

```js
const fullName = `${firstName} ${lastName}`;
```

---

### 8. Functions in Dependency Arrays
JavaScript

```js
function Search() {
  const fetchData = () => { /* logic */ };

  useEffect(() => {
    fetchData();
  }, [fetchData]);
}
```

The Pitfall: Effect runs every render.
The Explanation: Function identity changes each render.
The Fix: Remove or stabilize function.

```js
useEffect(() => {
  fetchData();
}, []);
```

---

### 9. Setting State from Props
JavaScript

```js
function Editor({ initialText }) {
  const [text, setText] = useState(initialText);
}
```

The Pitfall: Updates to props are ignored after first render.
The Explanation: useState only uses initial value once.
The Fix: Sync via effect.

```js
useEffect(() => {
  setText(initialText);
}, [initialText]);
```

---

### 10. Async Functions as useEffect Callback
JavaScript

```js
useEffect(async () => {
  const data = await fetch('/api');
  setData(data);
}, []);
```

The Pitfall: React throws warning/error.
The Explanation: useEffect must return void or cleanup function, not a Promise.
The Fix: Wrap async logic inside effect.

```js
useEffect(() => {
  const load = async () => {
    const data = await fetch('/api');
    setData(data);
  };

  load();
}, []);
```


### 11. Defining Components Inside Components
JavaScript

```js
function Parent() {
  const Child = () => <div>Hello</div>;

  return <Child />;
}
```

The Pitfall: Child component loses state and remounts every render.
The Explanation: A new component type is created on every render, so React treats it as a different component and remounts it.
The Fix: Define components outside.

```js
function Child() {
  return <div>Hello</div>;
}
```

---

### 12. Reading State Immediately After Setting It
JavaScript

```js
function update() {
  setCount(count + 1);
  console.log(count);
}
```

The Pitfall: Logs stale state.
The Explanation: State updates are asynchronous and batched.
The Fix: Compute value before setting.

```js
const next = count + 1;
setCount(next);
console.log(next);
```

---

### 13. The useMemo Trap
JavaScript

```js
const sortedList = useMemo(() => items.sort(), [items]);
```

The Pitfall: Performance worsens instead of improving.
The Explanation: Overhead of memoization outweighs computation cost.
The Fix: Use only for expensive computations or referential stability needs.

---

### 14. Missing Cleanup Functions
JavaScript

```js
useEffect(() => {
  window.addEventListener('resize', handleResize);
}, []);
```

The Pitfall: Memory leaks and duplicated listeners.
The Explanation: Event listeners persist after unmount.
The Fix: Cleanup subscription.

```js
useEffect(() => {
  window.addEventListener('resize', handleResize);

  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []);
```

---

### 15. Context Value Churn
JavaScript

```js
function App() {
  const [user, setUser] = useState(null);

  return (
    <AuthContext.Provider value={{ user, setUser }}>
      <Child />
    </AuthContext.Provider>
  );
}
```

The Pitfall: Unnecessary re-renders of all consumers.
The Explanation: New object reference triggers context update every render.
The Fix: Memoize value.

```js
const value = useMemo(() => ({ user, setUser }), [user]);
```

---

### 16. Mutating the DOM Directly
JavaScript

```js
function updateStyle() {
  document.getElementById('my-box').style.color = 'red';
}
```

The Pitfall: UI becomes inconsistent with React state.
The Explanation: React’s Virtual DOM gets out of sync with real DOM.
The Fix: Use state-driven UI or refs.

---

### 17. The useRef Render Trap
JavaScript

```js
const countRef = useRef(0);

function increment() {
  countRef.current++;
}

return <div>{countRef.current}</div>;
```

The Pitfall: UI does not update.
The Explanation: useRef updates do not trigger re-render.
The Fix: Use useState for UI values.

```js
setCount(prev => prev + 1);
```

---

### 18. Mutating Props
JavaScript

```js
function UserCard({ user }) {
  user.name = user.name.toUpperCase();

  return <div>{user.name}</div>;
}
```

The Pitfall: Parent state is mutated unexpectedly.
The Explanation: Props are read-only references.
The Fix: Derive new value.

```js
const upperName = user.name.toUpperCase();
```

---

### 19. Strict Mode Double Render Confusion
JavaScript

```js
let instanceCount = 0;

function MyComponent() {
  instanceCount++;
}
```

The Pitfall: Confusing double logs in development.
The Explanation: React StrictMode intentionally double-invokes renders in dev.
The Fix: Not a bug—move side effects into useEffect.

---

### 20. Over-fetching Race Conditions
JavaScript

```js
useEffect(() => {
  fetch(`/users/${id}`).then(res => setUser(res));
}, [id]);
```

The Pitfall: Out-of-order responses overwrite correct data.
The Explanation: Network responses resolve unpredictably.
The Fix: Cancel or ignore stale requests.

```js
useEffect(() => {
  let ignore = false;

  fetch(`/users/${id}`)
    .then(res => {
      if (!ignore) setUser(res);
    });

  return () => {
    ignore = true;
  };
}, [id]);
```

### 21. Batched setState Overrides
JavaScript

```js
function addTwo() {
  setCount(count + 1);
  setCount(count + 1);
}
```

The Pitfall: The count only increases by 1.
The Explanation: React batches state updates, and both calls use the same stale closure value.
The Fix: Use functional updates.

```js
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

---

### 22. Unnecessary React.memo Wrappers
JavaScript

```js
const Card = React.memo(({ children }) => {
  return <div>{children}</div>;
});
```

The Pitfall: No performance improvement, sometimes worse performance.
The Explanation: children and inline props change reference every render, breaking memoization.
The Fix: Only memoize components with stable primitive props.

---

### 23. Hydration Mismatches in SSR (Next.js/Remix)
JavaScript

```js
function Clock() {
  const time = Date.now();
  return <div>{time}</div>;
}
```

The Pitfall: Hydration error: "Text content did not match."
The Explanation: Server-rendered HTML differs from client render.
The Fix: Move dynamic values to client-only lifecycle.

---

### 24. Forgetting forwardRef on Custom Components
JavaScript

```js
function MyInput(props) {
  return <input />;
}

<MyInput ref={inputRef} />
```

The Pitfall: ref is undefined.
The Explanation: Function components do not accept refs by default.
The Fix:

```js
const MyInput = React.forwardRef((props, ref) => (
  <input ref={ref} />
));
```

---

### 25. Executing State Updates During Render
JavaScript

```js
function Counter({ count }) {
  if (count > 10) {
    setTheme('dark');
  }

  return <div />;
}
```

The Pitfall: "Cannot update a component while rendering another component."
The Explanation: State updates are not allowed during render phase.
The Fix: Move logic to useEffect or event handlers.

---

### 26. useState Lazy Initialization Mistake
JavaScript

```js
const [data, setData] = useState(computeHeavyData());
```

The Pitfall: computeHeavyData runs on every render.
The Explanation: Function is executed immediately before state initialization.
The Fix: Pass function reference.

```js
const [data, setData] = useState(computeHeavyData);
```

---

### 27. Returning Object as Cleanup Function
JavaScript

```js
useEffect(() => {
  const sub = api.subscribe();

  return { unsubscribe: sub.cancel };
}, []);
```

The Pitfall: React throws error.
The Explanation: Cleanup must be a function, not an object.
The Fix:

```js
return () => sub.cancel();
```

---

### 28. Assuming Arrays are Immutable
JavaScript

```js
const [list, setList] = useState([1, 2, 3]);

list.push(4);
setList(list);
```

The Pitfall: UI does not update.
The Explanation: Array reference does not change.
The Fix:

```js
setList([...list, 4]);
```

---

### 29. Default Props with Objects
JavaScript

```js
function Layout({ config = { theme: 'dark' } }) {
  useEffect(() => {}, [config]);
}
```

The Pitfall: Unnecessary re-renders or loops.
The Explanation: Default object is recreated every render.
The Fix:

```js
const defaultConfig = { theme: 'dark' };
```

---

### 30. JSX htmlFor / className Mistakes
JavaScript

```js
<label for="username">Username</label>
<input id="username" />
```

The Pitfall: React warning and accessibility issues.
The Explanation: JSX uses reserved JS keywords.
The Fix:

```js
<label htmlFor="username">Username</label>
<input id="username" />
```