## Web Accessibility (a11y)

Web accessibility (often abbreviated as **a11y**, representing the 11 letters between “a” and “y”) is the inclusive practice of designing and developing websites, tools, and technologies so that people with disabilities can use them effectively.

When built correctly, a website allows users to perceive, understand, navigate, and interact with content regardless of physical or cognitive impairments.

### Who Accessibility Benefits
Accessibility improves the experience for everyone, not only users with permanent disabilities. The **Persona Spectrum** of inclusive design shows that impairments fall into three categories:

- **Permanent:** Blindness, deafness, or missing a limb.
- **Temporary:** A broken arm (mouse navigation difficult) or an ear infection (hearing audio difficult).
- **Situational:** Holding a baby (one-handed navigation), bright sunlight (high color contrast needed), or a quiet library without headphones (video captions required).

### Core Technical Practices
Implementing accessibility bridges the gap between how a user physically interacts with a computer and how the browser interprets the code. The following practices form the foundation used by professional teams:

1. **Semantic HTML**  
   Use proper HTML elements for their intended purpose (`<button>` for actions, `<a>` for links, `<nav>`, `<main>`, `<header>`). This allows screen readers to understand structure and context.

   **Bad:**
   ```html
   <div class="button" onClick={submit}>Submit</div>
   ```
   (Screen reader sees only text in a box and does not know it is clickable.)

   **Good:**
   ```html
   <button onClick={submit}>Submit</button>
   ```
   (Screen reader announces it as a button and automatically maps Enter/Space keys.)

2. **Keyboard Navigation**  
   Every interactive element must be reachable via the Tab key in logical visual order. A highly visible focus indicator (usually an outline) must be present so sighted keyboard users always know their position.

3. **Visual Considerations**  
   - **Color Contrast:** Text and interactive elements must meet a minimum 4.5:1 ratio against their background (per WCAG).  
   - **Color as Information:** Never rely on color alone; pair it with icons or text (e.g., error states need both red borders and explanatory text).  
   - **Text Scaling:** Layouts must support up to 200% zoom without breaking or hiding content.

4. **Alt Text for Images**  
   Every informative image requires a meaningful `alt` attribute so screen readers can describe it. Decorative images use an empty `alt=""` so they are ignored.

   Example:
   ```html
   <img src="dog.jpg" alt="A Golden Retriever catching a frisbee" />
   ```

5. **ARIA (Accessible Rich Internet Applications)**  
   When native HTML is insufficient for complex custom components (e.g., tabbed interfaces or dropdowns), ARIA attributes provide extra context to screen readers.  

   **Note:** The first rule of ARIA is “No ARIA is better than bad ARIA.” Prefer native HTML elements whenever possible.

