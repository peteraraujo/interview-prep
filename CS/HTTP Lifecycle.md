## HTTP (Hypertext Transfer Protocol)

HTTP (Hypertext Transfer Protocol) is the foundational protocol of the World Wide Web. It defines the rules for transmitting data (HTML documents, images, videos, etc.) between clients and servers.

HTTP follows a **Client-Server** model and is **stateless**: every request is treated as an independent transaction. The server does not remember previous requests unless additional mechanisms (cookies, sessions, or tokens) are used.

### The HTTP Lifecycle (Journey of a Web Request)
When a user enters a URL (e.g., `https://www.example.com`) and presses Enter, the following sequence occurs in milliseconds:

1. **URL Parsing and DNS Resolution**  
   The browser parses the URL to extract the domain.  
   It checks its local cache for the corresponding IP address.  
   If not found, it performs a DNS (Domain Name System) lookup to translate the domain name into an IP address (e.g., `192.0.2.1`).

2. **TCP Handshake (Connection Establishment)**  
   HTTP relies on TCP (Transmission Control Protocol) for reliable delivery. The client and server perform a three-way handshake:  
   - **SYN**: Client sends a synchronization packet.  
   - **SYN-ACK**: Server acknowledges and agrees.  
   - **ACK**: Client confirms.  
   The TCP connection is now open.

3. **TLS/SSL Handshake (For HTTPS)**  
   For secure HTTPS connections, an additional handshake occurs immediately after TCP.  
   The client and server exchange cryptographic keys and verify digital certificates to establish encrypted communication.

4. **The HTTP Request**  
   The client sends an HTTP request containing:  
   - **Method**: Action to perform (e.g., `GET`, `POST`).  
   - **Path**: Resource being requested (e.g., `/index.html`).  
   - **Headers**: Metadata (user-agent, accepted languages, etc.).  
   - **Body** (optional): Data sent to the server (form fields, JSON, file uploads).

5. **The HTTP Response**  
   The server processes the request and returns an HTTP response containing:  
   - **Status Code**: Three-digit result (e.g., `200 OK`, `404 Not Found`, `500 Internal Server Error`).  
   - **Headers**: Metadata (content type, caching instructions, content length).  
   - **Body**: The actual payload (HTML, JSON, image data, etc.).

6. **Connection Teardown (or Keep-Alive)**  
   Historically (HTTP/1.0), the TCP connection was closed after the response.  
   Modern versions (HTTP/1.1 and HTTP/2) use **Keep-Alive** by default: the connection remains open for a short time so the browser can request additional resources (images, CSS, JavaScript) without repeating the expensive TCP and TLS handshakes.

