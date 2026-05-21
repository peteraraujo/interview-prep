## React Debugging Techniques (Data Flow + Lifecycle Focus)

---

### 1. React Developer Tools (The Profiler)

Relying solely on console.log in a large React app is a nightmare. You must install the React Developer Tools browser extension. It gives you two critical tabs: Components and Profiler.

The Concept: The Profiler allows you to record an interaction (like clicking a button) and see exactly which components re-rendered, how long they took, and most importantly, why they rendered.

The Practical Fix: If your app feels sluggish, open the Profiler, click the gear icon, and check "Record why each component rendered while profiling".

When you record a click, React will literally tell you: "Rendered because prop 'user' changed" or "Rendered because Hook 2 changed."

---

### 2. The useEffect Lifecycle Tracker

When a component is behaving erratically (fetching data twice, or state mysteriously resetting), it usually means the component is silently unmounting and remounting without you realizing it.

The Concept: You can use a dedicated useEffect with an empty dependency array to track the exact birth and death of a component in the console.

The Example:

```js
function BuggyComponent({ id }) {
  useEffect(() => {
    console.log(`[🟢 MOUNTED] BuggyComponent with ID: ${id}`);

    return () => {
      console.log(`[🔴 UNMOUNTED] BuggyComponent with ID: ${id}`);
    };
  }, [id]);

  return <div>Tracking...</div>;
}
```

If you see a red unmount log immediately followed by a green mount log, a parent component is accidentally destroying and recreating this component.

---

### 3. Tracking "Why Did You Render?"

Sometimes a component re-renders endlessly, and you cannot figure out which prop or state is triggering it. You can write a temporary custom hook to compare previous props to current props and log the exact culprit.

The Concept: By storing previous props in a useRef, we can detect referential changes.

The Example:

```js
function useTraceUpdate(props) {
  const prev = useRef(props);

  useEffect(() => {
    const changedProps = Object.entries(props).reduce((acc, [key, value]) => {
      if (prev.current[key] !== value) {
        acc[key] = [prev.current[key], value];
      }
      return acc;
    }, {});

    if (Object.keys(changedProps).length > 0) {
      console.log('Changed props:', changedProps);
    }

    prev.current = props;
  });
}

function MyComponent(props) {
  useTraceUpdate(props);
  return <div>UI</div>;
}
```

---

### 4. The debugger Keyword

When you have complex logic inside a useEffect or handler, console.log is often insufficient.

The Concept: The `debugger;` statement pauses execution in DevTools so you can inspect scope variables live.

The Example:

```js
function calculateTotal(cart) {
  const total = cart.reduce((sum, item) => {
    debugger;
    return sum + item.price;
  }, 0);

  return total;
}
```

---

### 5. Visualizing Render Cycles with CSS

Sometimes React DevTools is too heavy, and you want a fast visual signal for re-renders.

The Concept: Inject a dynamic style that changes every render to expose render frequency.

The Example:

```js
function SuspectComponent({ data }) {
  const randomColor =
    '#' + Math.floor(Math.random() * 16777215).toString(16);

  return (
    <div style={{ border: `4px solid ${randomColor}` }}>
      {data.text}
    </div>
  );
}
```

If the UI flashes constantly during typing or scrolling, the component is re-rendering excessively.

---

### 6. Catching Silent Crashes: Error Boundaries

If a deeply nested React component throws an unhandled error (like accessing undefined properties), React will unmount the entire UI tree.

The Concept: Error Boundaries catch rendering errors and display fallback UI instead of crashing the app.

The Example:

```js
import React from 'react';

class ErrorBoundary extends React.Component {
  state = { hasError: false, errorMessage: '' };

  static getDerivedStateFromError(error) {
    return { hasError: true, errorMessage: error.message };
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-box">
          Crashed: {this.state.errorMessage}
        </div>
      );
    }

    return this.props.children;
  }
}

function App() {
  return (
    <ErrorBoundary>
      <Dashboard />
    </ErrorBoundary>
  );
}
```