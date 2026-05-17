## React Best Practices

React is unopinionated and does not enforce a specific folder structure, state management solution, or routing library. This flexibility requires developers to follow established best practices to keep codebases scalable and maintainable.

### 1. Component Architecture & Design
- **Apply the Single Responsibility Principle (SRP):** A component should perform only one task. Large components that manage complex state, fetch data, and render extensive user interface (UI) should be broken into smaller, focused pieces. Complex logic should be extracted into custom hooks.
- **Separate Logic from Presentation:** User interface components should remain "dumb" (purely presentational) while logic stays in "smart" layers. Data-fetching or complex side effects should be extracted into custom hooks (for example, `useUserData()`), leaving the component to consume data and render only.
- **Destructure Props:** Destructuring makes component expectations explicit and improves readability.

  **Bad:**
  ```jsx
  function UserCard(props) {
    return <div>{props.user.name}</div>;
  }
  ```

  **Good:**
  ```jsx
  function UserCard({ user }) {
    return <div>{user.name}</div>;
  }
  ```

### 2. State Management
- **Colocate State:** State should live as close as possible to where it is used. Global stores or Context are unnecessary for localized values (for example, a modal-open boolean used only in the Header component should reside in the Header).
- **Separate Server State from Client State:** 
  - **Client State** (dark mode toggle, form input, active tab) is managed with `useState`, `useReducer`, Zustand, or Redux.
  - **Server State** (user profiles, product lists fetched from the backend) is never managed with `useState` or Redux. Dedicated libraries such as TanStack Query (React Query) or SWR handle caching, loading states, background refetching, and request deduplication automatically.

### 3. Performance & Optimization
- **Use the Correct `key` in Lists:** When rendering arrays with `.map()`, each item requires a stable, unique `key`. The array index must never be used as a key if the list can be reordered, filtered, or paginated, as this causes incorrect UI updates and performance issues. Always use a unique identifier from the data (for example, `item.id`).
- **Avoid Premature Memoization:** React is fast by default. Overuse of `useMemo`, `useCallback`, and `React.memo` adds memory overhead and can degrade performance. These hooks should be applied only when expensive calculations exist or when a child component re-renders excessively and causes measurable lag.

### 4. Working with Hooks
- **Follow the Rules of Hooks:** Hooks must be called only at the top level of React function components or custom hooks. They must never be placed inside loops, conditions, or nested functions.
- **Use `useEffect` for Synchronization, Not Actions:** The `useEffect` hook synchronizes the component with external systems (network, Document Object Model (DOM), or subscriptions). It is not an event handler.

  **Bad:** Triggering a POST request inside `useEffect` on button click.

  **Good:** Placing the POST request directly in the `onSubmit` event handler.
- **Always Include Exhaustive Dependencies:** Any variable (prop, state, or derived value) used inside `useEffect` must appear in the dependency array. Omitting dependencies to avoid infinite loops is incorrect; the proper fix is to stabilize the value or adjust where state updates occur.

