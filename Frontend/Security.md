## Security

### 1. Defend the DOM (Preventing Injection)

The primary goal is to ensure that user data is never treated as executable code.

- **Trusting the Framework (Usually):** Modern frameworks such as React, Vue, and Angular automatically escape string
  variables before rendering them to the Document Object Model (DOM). This default behavior should be relied upon
  whenever possible.
- **Sanitizing Raw HTML:** When raw HTML must be rendered (for example, displaying content from a WYSIWYG editor), the
  string is passed through a sanitizer such as DOMPurify before insertion via `dangerouslySetInnerHTML` or `v-html`.
- **Avoiding Dangerous JavaScript APIs:** The functions `eval()`, `setTimeout(string)`, and `setInterval(string)` are
  never used, as they execute strings as code and introduce significant security risks.

### 2. Lock Down Authentication and State

When the frontend is compromised, the attacker must not be able to obtain the user's identity.

- **Never Storing Tokens in localStorage:** `HttpOnly`, `Secure`, and `SameSite=Strict` cookies are used for sensitive
  session data and refresh tokens. This approach completely neutralizes Cross-Site Scripting (XSS) token theft.
- **Keeping Access Tokens Short-Lived:** Any in-memory Access Token is configured with an expiration of 10 to 15
  minutes.
- **Clearing State on Logout:** On logout, the cookie is deleted and the Redux/Zustand store or React state is
  explicitly wiped so that sensitive data does not remain in browser memory.

### 3. Fortify the Network Layer

The frontend must strictly control which external parties it communicates with and which parties are allowed to
communicate with it.

- **Implementing a Strict Content Security Policy (CSP):** The `Content-Security-Policy` header serves as the ultimate
  safety net. It is configured to explicitly allow scripts, images, and fonts only from trusted domains.
- **Enforcing HTTPS:** All traffic is encrypted. The `Strict-Transport-Security` (HSTS) header is used to instruct
  browsers to connect to the site exclusively via HTTPS.
- **Preventing UI Redressing (Clickjacking):** The `X-Frame-Options: DENY` header or the CSP `frame-ancestors 'none'`
  directive is applied so that malicious sites cannot embed the application inside an invisible iframe.

### 4. Manage the Supply Chain

Even when frontend code is secure, the thousands of Node Package Manager (NPM) packages in `node_modules` represent a
major attack vector.

- **Auditing Dependencies Routinely:** `npm audit` or `yarn audit` is executed as part of the Continuous
  Integration/Continuous Deployment (CI/CD) pipeline to identify known vulnerabilities in third-party libraries.
- **Locking Versions:** A `package-lock.json` or `yarn.lock` file is always maintained to guarantee that identical,
  verified versions of packages are deployed to production.
- **Using Subresource Integrity (SRI):** When a script is loaded from a Content Delivery Network (CDN) — such as a
  Google Font or analytics script — an `integrity` attribute containing a cryptographic hash is included in the
  `<script>` tag. This ensures the browser refuses to execute the script if it has been altered on the CDN server.

### 5. The "No Secrets" Rule

Everything shipped to the client is readable by the client.

- **Never Shipping Private API Keys:** Third-party secret keys (such as an Amazon Web Services (AWS) secret key or a
  Stripe private key) remain exclusively on the backend. When the frontend must interact with these services, it calls
  the backend, which then performs the authenticated request.
- **Hiding Business Logic:** Critical business rules (for example, calculating item prices in a shopping cart) are never
  enforced on the frontend. A user could open browser DevTools, modify the JavaScript, and alter values such as prices.
  The frontend exists only for display and user convenience; the backend remains the final authority.
