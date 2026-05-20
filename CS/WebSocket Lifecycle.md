## WebSocket Protocol

Unlike HTTP, where the client must always initiate requests, WebSocket is a persistent, full-duplex communication protocol. Once established, both the client and the server can send data to each other simultaneously at any time without re-establishing the connection.

This makes WebSockets ideal for real-time applications such as chat rooms, live sports tickers, and multiplayer games.

### The WebSocket Lifecycle
A WebSocket connection progresses through four distinct phases.

**1. Handshake (HTTP Upgrade)**  
WebSockets begin as a standard HTTP request, allowing them to pass through existing firewalls and proxies.  
The client sends an HTTP GET request with the headers `Connection: Upgrade` and `Upgrade: websocket`, plus a random `Sec-WebSocket-Key`.  
If the server supports WebSockets, it responds with HTTP status `101 Switching Protocols` and a `Sec-WebSocket-Accept` header (a cryptographic hash of the client’s key).  
At this point, the HTTP protocol is abandoned, and the underlying TCP connection switches permanently to the WebSocket protocol.

**2. Open State (Data Transfer)**  
Once open, communication has almost zero overhead.  
Data is sent in lightweight frames (header size is only 2–10 bytes), unlike HTTP’s heavy per-request headers.  
The connection is full-duplex, allowing the client and server to send messages simultaneously without waiting.

**3. Ping / Pong (Heartbeats)**  
Long-lived connections require periodic health checks.  
Either side sends a tiny control frame called a **Ping**.  
The other side must immediately reply with a **Pong** frame.  
If a Ping receives no Pong within the timeout, the connection is considered dead and is terminated.

**4. Closing Handshake**  
Closing is a two-way process to ensure no data is lost.  
Either the client or server initiates closure by sending a **Close** control frame (containing a status code such as 1000 for normal closure or 1006 for abnormal, plus an optional reason).  
The receiving side finishes any pending work and echoes a Close frame back.  
Only after the echo is received is the underlying TCP connection fully severed.

