### Cache First (Cache Falling Back to Network)

**Mechanism:** The system intercepts the request and checks the local cache. If the resource is present, it is returned immediately. If the resource is missing or expired, the system initiates a network request, stores the resulting response in the cache for future use, and then returns the response to the user.

* **Use Case:** Resources that are versioned or rarely change, where download speed is prioritized over immediate data freshness.
* **Examples:** Application logos, font files, compiled CSS stylesheets, and static JavaScript bundles.

---

### Network First (Network Falling Back to Cache)

**Mechanism:** The system routes the request directly to the network. If the network request succeeds, the system updates the local cache with the new data and returns the response. If the network request fails (e.g., due to lack of connectivity or server timeout), the system retrieves the most recent version of the data from the local cache.

* **Use Case:** Content that must be as up-to-date as possible, but still needs to remain accessible if the device loses internet connection.
* **Examples:** Inbox messages, user profile configurations, and frequently updated text articles.

---

### Cache Only

**Mechanism:** The system checks the local cache for the requested resource. If it is found, it is returned. If it is not found, the request fails entirely. The system makes no attempt to contact the network.

* **Use Case:** Specific application states where background data synchronization is disabled, or for accessing data explicitly pre-downloaded by the user.
* **Examples:** Accessing a predefined offline map area, or reading an article specifically downloaded via a "save for offline reading" function.

---

### Network Only

**Mechanism:** The system bypasses the local cache entirely, routing the request directly to the server. The response is not stored in the cache after retrieval.

* **Use Case:** Data that is highly sensitive, strictly ephemeral, or involves non-idempotent server mutations.
* **Examples:** Payment processing requests, administrative control panel actions, and logging analytics events.

---

### Stale-While-Revalidate

**Mechanism:** The system intercepts the request and immediately serves the cached version of the data to the user. Simultaneously, it initiates a background network request to fetch the most current version from the server. Once the new data is received, the cache is updated silently, ensuring the *next* request receives the newer data.

* **Use Case:** Content that requires immediate rendering for perceived performance, but where absolute, real-time data accuracy is not strictly necessary for the current view.
* **Examples:** Social media timeline feeds, weather forecast summaries, and user avatar images.