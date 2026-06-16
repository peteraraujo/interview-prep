### HTTP Headers

HTTP headers are fundamental components of the Hypertext Transfer Protocol (HTTP). They consist of case-insensitive key-value pairs transmitted at the beginning of an HTTP request or response. Their primary function is to pass additional information and metadata between the client (usually a web browser) and the server, dictating how a specific transaction should be processed, routed, or interpreted.

Below is a breakdown of how HTTP headers function and a categorization of the most common and strictly necessary headers used in web development.

---

### Structural Characteristics

* **Format:** A header consists of a name, followed immediately by a colon (`:`), a single space, and the associated value (e.g., `Host: www.example.com`).
* **Placement:** Headers immediately follow the initial request line (e.g., `GET /index.html HTTP/1.1`) or the initial response status line (e.g., `HTTP/1.1 200 OK`). They are separated from the main body payload by an empty line (a sequence of two carriage returns and line feeds).
* **Directionality:** Headers are broadly classified into Request Headers (sent by the client to the server) and Response Headers (sent by the server back to the client). Some headers can be used in both directions.

---

### Most Common and Useful Headers

The following headers dictate the core operational behavior of modern web applications.

#### Content Negotiation and Description

These headers ensure both the client and server understand the format of the data being transmitted.

* **`Content-Type`**: Indicates the exact media type of the resource in the HTTP body. It is required for POST and PUT requests.
* *Example:* `Content-Type: application/json` or `Content-Type: text/html; charset=UTF-8`


* **`Accept`**: Sent by the client to explicitly state which media types it is capable of processing and willing to receive as a response.
* *Example:* `Accept: text/html, application/xhtml+xml`


* **`Content-Length`**: Specifies the exact size of the message body, measured in decimal number of octets (bytes). The receiver relies on this to know when the transmission of the body is complete.

#### Client Identification and Routing

These headers provide necessary context about the origin of the request.

* **`Host`**: Specifies the domain name of the server being requested. This is the only header strictly mandated by the HTTP/1.1 specification. It allows a single physical server to host multiple domains at the same IP address.
* *Example:* `Host: api.example.com`


* **`User-Agent`**: Contains a characteristic string that allows the network protocol peers to identify the application type, operating system, and software version of the requesting client.
* *Example:* `User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/100.0.0.0`



#### Authentication and State Management

HTTP is a stateless protocol. These headers explicitly establish session continuity and user identity.

* **`Authorization`**: Contains the credentials required to authenticate a user agent with a server. It typically includes an authentication type and a cryptographic token.
* *Example:* `Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5c...`


* **`Cookie`**: Sent by the client in a request, containing stored HTTP cookies previously assigned by the server. Used for session tracking.
* **`Set-Cookie`**: Sent by the server in a response to instruct the client to store a specific key-value pair and send it back in future requests.

#### Caching and Performance

These headers control how resources are stored locally by the browser or intermediate proxy servers to reduce redundant network requests.

* **`Cache-Control`**: The primary header for dictating caching rules. It contains specific directives regarding who can cache the response and for how long.
* *Example:* `Cache-Control: public, max-age=3600` (Allows any cache to store the file for one hour).


* **`ETag`**: An identifier (often a hash) representing a specific version of a resource. The client uses this in subsequent requests to ask the server if the file has changed, avoiding a full download if the ETag matches.

#### Security Policies

These headers are sent by the server to enforce strict security constraints on the client browser.

* **`Strict-Transport-Security` (HSTS)**: Forces the browser to exclusively communicate with the server using encrypted HTTPS connections, preventing unencrypted HTTP downgrades.
* **`Access-Control-Allow-Origin`**: The foundational header of Cross-Origin Resource Sharing (CORS). It indicates whether the response can be shared with requesting code from a specified external domain.
* **`Content-Security-Policy` (CSP)**: Dictates the approved, trusted sources from which the browser is permitted to load executable scripts and resources, neutralizing script injection vulnerabilities.