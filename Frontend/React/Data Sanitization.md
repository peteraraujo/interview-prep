### React Input Data Sanitization
Input data sanitization is the technical process of inspecting and cleaning untrusted user data before an application processes, stores, or displays it. In a React application, the primary objective of sanitization is to prevent Cross-Site Scripting (XSS) attacks, where malicious executable scripts are injected into the user interface.

Below is a breakdown of how React inherently handles untrusted data, the specific vulnerabilities that remain, and the required methods for mitigating them.

---

### Native React Protections

React includes a default security mechanism to neutralize standard script injection attempts.

* **Automatic Escaping:** When data is bound to the interface using standard JSX syntax (e.g., `<div>{userInput}</div>`), the React DOM automatically converts the input values into strings before rendering them.
* **Text Node Conversion:** If a user submits a string containing HTML or JavaScript (such as `<script>alert(1)</script>`), React does not parse the string as structural code. It renders the input strictly as a visible text node. The browser displays the exact characters on the screen, but the underlying script is not executed.

### Direct Vulnerabilities in React

Despite native escaping, developers can intentionally or accidentally bypass these protections, creating vulnerabilities that require explicit sanitization.

#### 1. dangerouslySetInnerHTML

React provides the `dangerouslySetInnerHTML` property to allow developers to render raw HTML strings directly into the Document Object Model.

* **The Risk:** If untrusted user input is passed directly into this property, React bypasses its automatic escaping mechanism. The browser will parse and execute any malicious script tags contained within the input.
* **The Requirement:** Data passed to this property must be strictly sanitized.

#### 2. URL Attribute Injection

React does not automatically sanitize strings placed into URL-based attributes, such as the `href` attribute of an anchor tag (`<a>`) or the `src` attribute of an image or iframe.

* **The Risk:** An attacker can provide a URL prefixed with the `javascript:` protocol (e.g., `javascript:executeMaliciousCode()`). If this string is passed to an `href` attribute, the script will execute immediately when the user clicks the link.

### Required Sanitization Implementations

To address the vulnerabilities mentioned above, developers must implement active sanitization procedures within the client-side code.

* **HTML Sanitization Libraries:** When using `dangerouslySetInnerHTML` is unavoidable (such as rendering content from a rich text editor), developers must process the HTML string through a dedicated sanitization library before rendering it. Libraries like DOMPurify parse the HTML string, identify executable scripts or malicious attributes, strip them out, and return a safe HTML string containing only permitted formatting elements.
* **URL Protocol Validation:** Before rendering a user-provided URL into an `href` or `src` attribute, the string must be parsed and validated. The application must verify that the URL begins with a safe, expected protocol, strictly limiting acceptable values to `https://`, `http://`, or `mailto:`. Any URL failing this check must be rejected or neutralized.

### Client-Side vs. Server-Side Execution

It is a strict architectural rule that client-side sanitization in React is exclusively a display-level defense.

Because all React code is executed within the user's browser, the sanitization logic can be bypassed if an attacker sends an HTTP request directly to the backend database, circumventing the React interface entirely. Therefore, all data must also be independently validated and sanitized by the backend server before it is written to the database, ensuring the stored data is inherently safe before it is ever sent back to the React application.