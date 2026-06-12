### React Fiber: The Internal Update Engine

React Fiber is the internal algorithm React uses to calculate and manage updates to a user interface. Introduced in React 16, it completely replaced React's original rendering engine. The core purpose of Fiber is to make the rendering process interruptible, allowing React to prioritize different types of updates and prevent the browser from freezing during complex calculations.

---

### The Problem Fiber Solves

To understand Fiber, it is necessary to look at how React operated before its introduction.

Originally, React processed updates synchronously. When a component’s state changed, React would begin calculating the necessary updates for that component and all of its descendants in one continuous, unbroken operation.

The browser running the application has a single main thread that handles JavaScript execution, user interactions, and screen repaints. Because the original React engine could not be stopped once it started, a complex update involving thousands of components would occupy the main thread until it finished. If the user tried to type in an input field or scroll the page during this time, the browser would be unresponsive, resulting in visible lag or dropped frames.

React Fiber solves this by allowing React to pause its calculations, check if the browser has more urgent tasks to perform, and then resume its work.

---

### How Fiber Works

Fiber changes the architecture of React from a continuous execution model to a chunked execution model. It achieves this through three main mechanisms:

**1. Units of Work (Fibers)**
React breaks down the entire application tree into individual, discrete units of work. Each of these units is called a "fiber." A fiber corresponds to a specific React component and contains information about that component's state, props, and the work that needs to be done.

**2. Pausing and Yielding**
Instead of processing every component at once, React processes a single fiber, completes the work for that component, and then explicitly checks the status of the browser's main thread. If the browser needs to handle a high-priority event—such as a user clicking a button or a CSS animation needing to run—React pauses its rendering process and yields control back to the browser. Once the browser finishes the urgent task, React resumes processing the next fiber exactly where it left off.

**3. Prioritization**
Because the work is broken into individual fibers, React can assign different priority levels to different updates:

* **High Priority:** Updates triggered by direct user interaction, such as typing or clicking. These must be processed immediately to keep the application feeling responsive.
* **Low Priority:** Updates triggered by background processes, such as data arriving from a network request. These can be delayed slightly without the user noticing.

If React is in the middle of processing a low-priority update and a high-priority update occurs, it can abandon the current work, process the high-priority update immediately, and restart the low-priority work afterward.

---

### The Two Phases of Execution

To ensure the user never sees a partially updated screen, Fiber divides the update process into two strict phases:

* **The Render Phase:** During this phase, React traverses the fibers and calculates all the changes that need to be made to the screen. This phase is entirely invisible to the user and is strictly interruptible. React can pause, discard, or restart the work in this phase at any time to prioritize other tasks.
* **The Commit Phase:** Once React has calculated all the necessary changes, it moves to the commit phase. In this phase, React applies the calculated changes to the actual browser environment. This phase cannot be interrupted. It executes synchronously to ensure the user interface updates consistently all at once.

In summary, React Fiber is an internal restructuring that allows React to split UI updates into individual tasks, prioritize them based on user interaction, and pause background work to keep the browser responsive.