### HTTP Caching

HTTP caching is a standard protocol mechanism designed to store copies of web resources (such as HTML documents, images, and JavaScript files) after they are initially downloaded. When a subsequent request is made for the same resource, the system can serve the stored copy rather than requesting it again from the origin server. The primary functions of HTTP caching are to decrease network traffic, reduce server computational load, and minimize the latency experienced by the end user.

---

### Classifications of HTTP Caches

Caches are structurally divided into two main categories based on where they are located and who has access to the stored data.

* **Private Caches:** These caches are dedicated to a single user and are built directly into the user's web browser. They store resources locally on the user's hard drive or in active memory. Private caches ensure that when a user navigates backward or reloads a page, previously downloaded assets are retrieved instantly without network transmission.
* **Shared Caches:** These caches sit between the client and the origin server and store responses to be reused by multiple different users. Shared caches are typically implemented by Internet Service Providers (ISPs), corporate network proxies, or Content Delivery Networks (CDNs). They intercept requests from various clients; if a requested file is already stored in the shared cache, the proxy serves it directly, preventing the request from ever reaching the origin server.

### The Cache-Control Header

The behavior of both private and shared caches is governed primarily by the `Cache-Control` HTTP header, which is attached to the response sent by the origin server. This header contains specific directives that dictate exactly how and for how long a resource may be stored.

* **`max-age`:** Specifies the maximum amount of time, in seconds, that a cached resource is considered valid (fresh). Once this time elapses, the resource is considered "stale" and must be verified or re-downloaded.
* **`no-store`:** Instructs the cache that it must not store any part of the request or response under any circumstances. This is mandatory for highly sensitive data, such as banking information or personal records.
* **`no-cache`:** A frequently misunderstood directive. It does not prohibit caching. Instead, it instructs the cache that it must submit a validation request to the origin server before serving the stored copy to the user, ensuring the data has not changed.
* **`public` vs. `private`:** `public` indicates that any cache (browser or shared CDN) may store the response. `private` indicates that the response is intended for a single user and must only be stored in a private browser cache, preventing a CDN from serving one user's account data to another user.

### Validation Mechanisms

When a cached resource exceeds its `max-age`, it becomes stale. Before deleting it and downloading the entire file again, caches use conditional requests to ask the server if the underlying file has actually been modified.

There are two primary mechanisms for this validation:

#### 1. Entity Tags (ETags)

An ETag is a unique identifier (typically a hash of the file's contents) assigned by the server.

* When the server first delivers a file, it includes the `ETag` header.
* When the cache's copy becomes stale, the browser sends a conditional request containing the `If-None-Match` header, passing the stored ETag back to the server.
* The server compares the client's ETag with the ETag of the current file. If they match, the server returns an HTTP 304 (Not Modified) status code without a body, instructing the cache to reset the `max-age` timer and use the existing local file.

#### 2. Last-Modified Dates

This is a time-based validation method.

* The server includes a `Last-Modified` header indicating the exact date and time the file was last altered.
* When the cache needs to validate the file, it sends a conditional request with the `If-Modified-Since` header.
* The server checks the file's current modification date against the provided date. If the file has not been altered since that date, it returns an HTTP 304 status code, confirming the cached copy is still accurate.