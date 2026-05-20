## Critical Rendering Path (CRP)

The Critical Rendering Path (CRP) is the sequence of steps the browser follows to convert HTML, CSS, and JavaScript into pixels on the screen. Optimizing the CRP is the foundation of web performance because it directly determines how quickly a page becomes visible to the user.

The process consists of five distinct phases:

### 1. Constructing the DOM (Document Object Model)
The browser receives raw HTML and translates it into a tree structure it can understand.

- Reads raw bytes and converts them to characters based on the file encoding (typically UTF-8).
- Tokenizes the HTML (identifying start tags, end tags, and text content).
- Converts tokens into nodes (objects representing each element).
- Links nodes into a tree structure that captures parent-child relationships.

### 2. Constructing the CSSOM (CSS Object Model)
While parsing HTML, the browser downloads and parses linked CSS stylesheets.

- CSS is read, tokenized, and converted into its own independent tree structure (CSSOM).
- **Crucial Note:** CSS is a render-blocking resource. The browser will not render any content until the CSSOM is fully constructed, preventing a flash of unstyled content (FOUC).

### 3. Building the Render Tree
Once both the DOM and CSSOM are ready, the browser combines them into a single Render Tree.

- Only includes nodes that are actually visible on the screen.
- Traverses the DOM and matches each visible node with its corresponding CSS rules from the CSSOM.
- Excludes non-visual elements such as `<head>`, `<meta>`, and `<script>`.
- Omits elements hidden with `display: none` (elements with `visibility: hidden` are still included because they occupy space).

### 4. Layout (Reflow)
With the Render Tree complete, the browser calculates the exact geometry of every element.

- Determines the precise size and position of each node within the viewport.
- Computes the Box Model (width, height, margins, padding) for every visible element based on the current screen size and device.

### 5. Paint
The final step converts the Render Tree and Layout information into actual pixels.

- Draws text, colors, images, borders, and shadows.
- Often uses multiple layers (compositing) to handle overlapping elements such as dropdowns or sticky headers correctly.

Understanding the CRP is essential for performance optimization. Techniques such as minimizing render-blocking resources, deferring non-critical JavaScript, and prioritizing above-the-fold CSS all aim to shorten the time the browser spends navigating this path.

