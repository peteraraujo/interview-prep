### The Rendering Pipelines: React Web vs. React Native

To understand how React and React Native differ, it is necessary to separate the logic of calculating a user interface from the act of drawing it on a screen. React itself does not draw pixels. It is a calculation engine. Because of this separation, the first half of the rendering process is identical for both web and mobile, while the second half diverges completely based on the host environment.

Here is the step-by-step breakdown of how data becomes a visible interface on both platforms.

---

### Initial Path: The Shared JavaScript Core

Regardless of whether an application is running in a web browser or on a smartphone, the initial steps happen entirely within the JavaScript runtime.

1. **State Change:** A user interaction or network response updates the data within a component.
2. **Component Execution:** React executes the JavaScript functions for the components affected by the data change. These functions return a description of what the user interface should look like.
3. **Virtual Tree Generation:** React takes those descriptions and builds a lightweight tree of JavaScript objects. This represents the new desired state of the user interface.
4. **Reconciliation:** React compares this newly generated tree against the previous version of the tree. It identifies the exact differences between the two.
5. **Instruction Output:** React generates a strict list of instructions detailing what needs to be added, modified, or removed from the screen.

At this exact point, the core React library finishes its job. It hands the list of instructions over to a platform-specific renderer.

---

### Path A: The Web Pipeline (React DOM)

If the application is running in a web browser, the `react-dom` package takes over to process the instructions.

1. **DOM Mutation:** `react-dom` translates React's instructions into standard browser API commands. It directly updates the Document Object Model (DOM), which is the browser's internal structure of the web page.
2. **Style Calculation:** The browser evaluates the CSS rules applied to the elements in the DOM.
3. **Layout:** The browser calculates the exact geometry of the page. It determines how much space each text block, image, and container requires, and where they should be positioned relative to one another.
4. **Paint:** The browser's graphics engine draws the actual pixels onto the computer monitor or phone screen.

---

### Path B: The Mobile Pipeline (React Native)

If the application is running as a mobile app, there is no browser and no Document Object Model. The `react-native` package takes over to process the instructions.

1. **Shadow Tree Creation:** The JavaScript thread passes the instructions to a C++ layer. This layer constructs a "Shadow Tree," which is a C++ data structure that mirrors the structure of the React components.
2. **Layout Calculation:** Native mobile operating systems (iOS and Android) do not understand CSS. To solve this, React Native uses a C++ layout engine called Yoga. Yoga reads the styling rules from the Shadow Tree and calculates the exact numerical coordinates (X, Y, width, and height) for every single element on the screen.
3. **Native UI Instantiation:** React Native passes these calculated coordinates to the device's native operating system. It instructs the OS to create actual, native user interface elements—such as a standard iOS `UIView` or an Android `ViewGroup`—and place them at those specific coordinates.
4. **Paint:** The iOS or Android native graphics thread takes over and draws the native elements onto the device screen.

---

### Summary of the Split

The rendering pipeline is a two-part system. The first part is environment-agnostic: React uses JavaScript to calculate what needs to change. The second part is environment-specific: a renderer takes those calculations and uses the host system's native tools (either browser APIs or mobile OS APIs) to physically draw the interface.