### Content Security Policy (CSP)

Content Security Policy (CSP) is a computer security standard enforced by web browsers. It is implemented primarily as an HTTP response header. Its function is to allow web server administrators to explicitly declare the approved, trusted sources from which a browser is permitted to load resources, such as JavaScript, CSS, and images.

The primary objective of CSP is to mitigate the impact of Cross-Site Scripting (XSS) and data injection vulnerabilities. By restricting resource origins, CSP prevents the browser from executing malicious scripts injected into the webpage by an attacker.

---

### Mechanism of Action

CSP operates on a strict allowlist model. When a browser receives the `Content-Security-Policy` header alongside an HTML document, it parses the rules defined in the header.

Before the browser downloads a resource or executes a script, it checks the resource's origin against the defined policy. If the origin is explicitly listed in the policy, the browser processes the resource. If the origin is absent from the policy, the browser blocks the network request, suppresses the execution of the code, and throws an error in the developer console.

### Core Policy Directives

A CSP is constructed using specific directives, each governing a distinct type of web resource. Multiple directives are separated by semicolons.

* **`default-src`**: Serves as the fallback directive for most other resource types. If a specific directive (like `font-src`) is not defined in the policy, the browser applies the rules defined in `default-src`.
* **`script-src`**: The most critical directive for XSS mitigation. It dictates exactly where JavaScript can be loaded from. By default, an active CSP disables the execution of inline scripts (code written directly inside HTML `<script>` tags) and the use of the `eval()` function, requiring developers to move all JavaScript into separate, explicitly trusted `.js` files.
* **`style-src`**: Restricts the origins of CSS stylesheets. Like `script-src`, it disables inline `<style>` blocks and inline `style=""` HTML attributes by default.
* **`connect-src`**: Restricts the URLs to which the client-side JavaScript can make network connections using APIs like `fetch`, `XMLHttpRequest`, or WebSockets.
* **`img-src`**: Dictates the permitted origins for loading images and favicons.

### Implementation Methods

There are two primary methods for delivering a CSP to the client browser:

1. **HTTP Response Header:** The standard and most secure implementation. The web server attaches the header to the outgoing HTTP response (e.g., `Content-Security-Policy: default-src 'self'; script-src https://trusted-cdn.com;`).
2. **HTML Meta Tag:** If a developer lacks access to server configuration, a CSP can be defined within the HTML document's `<head>` using a `<meta>` tag (e.g., `<meta http-equiv="Content-Security-Policy" content="default-src 'self'">`). This method lacks support for certain reporting features available to the HTTP header.

### Deployment and Reporting

Because a strict CSP can easily break a functional website by blocking legitimate resources, the standard provides mechanisms for safe deployment and monitoring.

* **Reporting Directives (`report-uri` / `report-to`)**: These directives instruct the browser to send an automated JSON report to a designated server endpoint whenever a policy violation occurs. This provides administrators with real-time telemetry regarding blocked resources and potential attacks.
* **Report-Only Mode**: Before actively blocking content, administrators can use the `Content-Security-Policy-Report-Only` HTTP header. In this mode, the browser evaluates the policy and sends violation reports to the specified endpoint, but it does not actually block the resources. This allows developers to monitor the policy in a production environment, identify misconfigurations, and adjust the directives before enforcing the blocking behavior.