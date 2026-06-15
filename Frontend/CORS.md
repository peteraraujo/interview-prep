### CORS: A Factual Overview

Cross-Origin Resource Sharing (CORS) is an HTTP-header-based mechanism that allows a web server to indicate the external origins from which a web browser should permit the loading of resources. It operates as a controlled relaxation of the browser's default security model.

---

### The Same-Origin Policy (SOP)

To understand CORS, one must first understand the restriction it modifies: the Same-Origin Policy.

The SOP is a security mechanism enforced by all modern web browsers. It dictates that a web application running in a browser can only request data from the exact same origin that served the application itself.

An "origin" is strictly defined by three components:

1. **Protocol** (e.g., `https://`)
2. **Domain** (e.g., `api.example.com`)
3. **Port** (e.g., `:443`)

If a script hosted on `https://website.com` attempts to make an `XMLHttpRequest` or `fetch` call to `https://api.external.com`, the browser will execute the request, receive the response, but block the JavaScript from reading that response because the domains do not match. This prevents malicious scripts from silently extracting data from authenticated sessions on other domains.

### How CORS Operates

CORS provides a standardized way for servers to tell the browser: *"It is acceptable to bypass the Same-Origin Policy for this specific request."* It is important to note that CORS is enforced entirely by the client-side browser, not the server. The server's role is strictly to append the correct HTTP headers to its responses. If the browser does not see the required headers in the server's response, it blocks the application code from accessing the data.

The CORS specification divides HTTP requests into two distinct categories, which the browser handles differently.

#### 1. Simple Requests

A simple request typically uses a `GET`, `POST`, or `HEAD` method and does not include custom HTTP headers (such as `Authorization` or `Content-Type: application/json`).

* **Execution:** The browser sends the HTTP request directly to the cross-origin server. It automatically attaches a header identifying the caller (e.g., `Origin: https://website.com`).
* **Validation:** The server processes the request and sends the data back. Crucially, the server must include the `Access-Control-Allow-Origin` header in its response.
* **Enforcement:** The browser receives the response and checks the header. If the header matches the requesting origin, the browser hands the data to the JavaScript. If the header is missing or does not match, the browser throws a CORS error and suppresses the data.

#### 2. Preflight Requests

If a request uses methods that can modify server data (such as `PUT`, `PATCH`, or `DELETE`) or includes custom headers, the browser considers it a "complex" request. Because these requests could execute destructive actions, the browser refuses to send the actual request until it verifies the server permits it.

* **The Preflight:** Before sending the actual request, the browser automatically pauses and sends an HTTP `OPTIONS` request to the target URL. This request contains no body data. It includes headers asking the server for permission (e.g., `Access-Control-Request-Method: DELETE`).
* **The Server Approval:** The server must respond to this `OPTIONS` request with HTTP headers detailing exactly what methods and headers are permitted for that endpoint.
* **The Actual Request:** If the browser confirms that the server's allowed methods and headers match what the JavaScript intends to send, the browser finally transmits the actual HTTP request. If the server denies the preflight, the actual request is never sent.

### Primary CORS Headers

The configuration of CORS relies on specific HTTP headers returned by the server.

* **`Access-Control-Allow-Origin`:** Dictates which origin is permitted to read the response. It can specify a single explicit URI or use a wildcard (`*`) to allow any origin.
* **`Access-Control-Allow-Methods`:** Used in response to a preflight request to indicate the HTTP methods (e.g., `GET, POST, PUT`) permitted at the endpoint.
* **`Access-Control-Allow-Headers`:** Used in response to a preflight request to indicate which custom HTTP headers can be used during the actual request.
* **`Access-Control-Allow-Credentials`:** A boolean flag indicating whether the browser is permitted to expose the response to the frontend code when the request included credentials (such as cookies or HTTP authentication). If credentials are used, the server cannot use a wildcard (`*`) for the allowed origin; it must specify the exact requesting origin.