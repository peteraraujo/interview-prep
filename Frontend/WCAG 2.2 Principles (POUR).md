## WCAG 2.2 Principles (POUR)



The Web Content Accessibility Guidelines (WCAG) 2.2 are built upon four foundational principles that determine whether web content is accessible to individuals with disabilities. These principles are commonly referred to by the acronym **POUR**. If a digital interface fails to meet any of these four principles, it becomes unusable for people relying on assistive technologies or alternative navigation methods.







### 1\. Perceivable



Information and user interface components must be presentable to users in ways they can perceive. Content cannot be invisible to all of a user's senses; it must be available through sight, hearing, or touch.



* **Core Concepts:** Provide text alternatives for non-text content (images and charts), offer captions or audio descriptions for video, and ensure content can be presented in different ways (such as simpler layouts) without losing information.
* **WCAG 2.2 Enhancements:** The update emphasizes visibility by ensuring that when an element receives keyboard focus, the focus indicator is highly visible and is never entirely obscured by sticky headers or author-created content (`Focus Not Obscured`, `Focus Appearance`).







### 2\. Operable



User interface components and navigation must be operable. The interface cannot require an interaction method that a user cannot perform.



* **Core Concepts:** Make all functionality available from a keyboard, provide users enough time to read and use content, avoid design choices that could cause seizures (such as rapid flashing), and provide clear ways to navigate and find content.
* **WCAG 2.2 Enhancements:** The newest criteria focus heavily on motor accessibility. Interactive elements must have a sufficient minimum physical size to be easily clicked or tapped (`Target Size`). Any action requiring a dragging movement (such as drag-and-drop) must be achievable through a simpler single-pointer click or tap alternative (`Dragging Movements`).







### 3\. Understandable



Information and the operation of the user interface must be understandable. Users must be able to comprehend the text and naturally figure out how to operate the interface.



* **Core Concepts:** Ensure text is readable and legible, make web pages appear and operate in predictable ways, and help users avoid and correct mistakes during form submissions.
* **WCAG 2.2 Enhancements:** This version introduces significant protections for users with cognitive disabilities. Interfaces must not ask users to re-enter information they have already provided in the same process (`Redundant Entry`). Contact details or help mechanisms must remain in the exact same location across multiple pages (`Consistent Help`). Logging in must not force users to solve puzzles or memorize passwords without the help of a password manager or alternative method (`Accessible Authentication`).







### 4\. Robust



Content must be robust enough to be interpreted reliably by a wide variety of user agents, including current and future assistive technologies.



* **Core Concepts:** This principle is strictly technical. As technologies and browsers evolve, the content should remain accessible. This is achieved by writing valid, semantic HTML and properly utilizing Accessible Rich Internet Applications (ARIA) attributes. All custom interactive components must expose a clear "Name, Role, and Value" so screen readers and voice-control software can accurately interpret and interact with the digital environment.

