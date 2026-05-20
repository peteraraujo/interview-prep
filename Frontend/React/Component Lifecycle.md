## React Component Lifecycle



The React component lifecycle represents the series of events that occur from the moment a component is created and
inserted into the Document Object Model (DOM) until it is removed. While React has transitioned heavily toward
functional components with Hooks, the underlying lifecycle concepts remain identical to the older class-based
architecture.

Understanding this lifecycle requires separating it into **Stages** (the high-level events of a component's existence)
and **Execution Phases** (the exact steps React takes during those events).



### Execution Phases

Every time a component is processed, React moves through three distinct phases:

* **Render Phase:** React calls the component to calculate what the UI should look like based on current state and
props. This phase is pure and produces a Virtual DOM representation. No actual DOM changes occur here.
* **Commit Phase:** React takes the differences calculated in the Render Phase and physically applies them to the
browser's DOM.
* **Effect Phase:** Immediately following the Commit Phase, React runs side effects (such as data fetching,
subscriptions, or manual DOM manipulations).



### Lifecycle Stages Breakdown

The lifecycle of any React component — whether class-based or functional — flows through three primary stages: Mounting,
Updating, and Unmounting.





#### 1\. Mounting (Creation)



Mounting occurs when a component is initialized and added to the DOM for the first time.

* **Render Phase:**

  * Functional Components: The entire function body executes to establish the initial state and return the initial
JSX.
  * Class Components: The constructor runs to set initial state, followed by the `render()` method returning the
initial JSX.
* **Commit Phase:** React creates the actual DOM nodes and inserts them into the document.
* **Effect Phase:**

  * Functional Components: React synchronously runs `useLayoutEffect` setups before the browser paints, followed
asynchronously by `useEffect` setups after the browser paints.
  * Class Components: The `componentDidMount()` lifecycle method is invoked immediately after the component is
inserted into the tree. This is the standard location for initial network requests or subscriptions.





#### 2\. Updating (Re-rendering)



Updating happens when a component needs to re-render due to changes in its props, its internal state, or a re-render of
its parent component.

* **Render Phase:**

  * Functional Components: The function body executes again with the newly updated state or props.
  * Class Components: The static `getDerivedStateFromProps()` method runs, followed by `shouldComponentUpdate()` (
which determines if the render should proceed). Finally, the `render()` method calculates the new Virtual DOM.
* **Commit Phase:** React compares the new Virtual DOM with the old one (reconciliation) and updates only the changed
DOM nodes in the browser.

  * Class Components (Pre-Commit): `getSnapshotBeforeUpdate()` captures information from the DOM (such as scroll
position) right before it changes.
* **Effect Phase:**

  * Functional Components: React runs the cleanup functions from the previous render's effects, then runs the setup
functions for the current render's effects.
  * Class Components: The `componentDidUpdate()` method is invoked, allowing developers to operate on the DOM after an
update or trigger new network requests based on prop changes.





#### 3\. Unmounting (Deletion)



Unmounting is the final stage, occurring when a component is completely removed from the DOM. This stage is critical for
cleaning up memory and preventing leaks.

* **Render Phase:** N/A (The component is being destroyed, not recalculated).
* **Commit Phase:** React removes the component's nodes from the browser's DOM.
* **Effect Phase (Cleanup):**

  * Functional Components: React runs the cleanup functions returned by the component's `useEffect` or
`useLayoutEffect` hooks.
  * Class Components: The `componentWillUnmount()` method is called right before the component is destroyed. This is
where timers are cleared, network requests are canceled, and event listeners are removed.





### Lifecycle Mapping Summary



To clearly see how both paradigms align with the core lifecycle stages, consider the following mapping:



|Lifecycle Stage|Execution Phase|Class Component Method|Functional Component (Hooks)|
|-|-|-|-|
|Mounting|Render|`constructor`, `render()`|Function body execution|
|Mounting|Effect|`componentDidMount()`|`useEffect(..., \[])` setup|
|Updating|Render|`render()`|Function body execution|
|Updating|Effect|`componentDidUpdate()`|`useEffect(..., \[deps])` cleanup \& setup|
|Unmounting|Effect (Cleanup)|`componentWillUnmount()`|`useEffect` return function|





By structuring applications around these three stages and understanding when React executes side effects versus
rendering logic, developers ensure that components remain performant and free of memory leaks regardless of the syntax
used.

