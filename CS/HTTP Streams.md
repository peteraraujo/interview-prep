## HTTP Streaming

HTTP Streaming allows a server to send an HTTP response in multiple smaller pieces (**chunks**) as soon as each piece is ready, while keeping the connection open until the entire transmission is complete. This contrasts with standard HTTP, which is strictly buffered: the server gathers all data, packages it, and sends it in one complete response.

This is the technology behind Large Language Model responses (word-by-word generation), large file downloads, and live data feeds.

### Why HTTP Streaming is Necessary
Standard HTTP forces users to wait for the full payload, leading to poor **Time to First Byte (TTFB)**. Streaming solves three major problems:

- **Immediate Feedback:** The client can begin rendering HTML, playing video, or displaying text as soon as the first chunk arrives.
- **Memory Efficiency:** The server never needs to load the entire payload (e.g., a 500 MB file) into RAM. It streams data incrementally.
- **Unknown Payload Sizes:** Ideal for dynamic content where the final size is unknown (live camera feeds, AI generation).

### How It Works Under the Hood
- The server omits the `Content-Length` header.
- It uses `Transfer-Encoding: chunked` (HTTP/1.1) or native multiplexing (HTTP/2 and HTTP/3).
- Each chunk is sent with its size prefix.
- The server ends the stream with a special **zero-length chunk**, signaling completion to the client.

### Consuming Streams in Frontend JavaScript
You cannot use `await response.json()` because it waits for the full body. Instead, use the **Streams API** (`ReadableStream`) to process chunks incrementally.

```jsx
async function fetchStreamedData() {
  const response = await fetch('https://api.example.com/generate-report');

  const reader = response.body.getReader();
  const decoder = new TextDecoder('utf-8');

  while (true) {
    const { done, value } = await reader.read();

    if (done) {
      console.log('Stream completely finished!');
      break;
    }

    const chunkText = decoder.decode(value);
    console.log('Received chunk:', chunkText);
    // Example: setOutput(prev => prev + chunkText);
  }
}
```

### Streaming vs Server-Sent Events (SSE) vs WebSockets
| Feature          | HTTP Streaming                          | SSE (Server-Sent Events)               | WebSockets                              |
|------------------|-----------------------------------------|----------------------------------------|-----------------------------------------|
| Direction        | Unidirectional (server → client)       | Unidirectional (server → client)       | Bidirectional (full-duplex)             |
| Use Case         | Large responses, AI generation, file downloads | Live updates, notifications            | Real-time chat, multiplayer games       |
| Protocol         | Standard HTTP request/response         | HTTP with `text/event-stream`          | Dedicated WebSocket protocol            |
| Connection       | One request, streaming response        | Long-lived HTTP connection             | Persistent TCP connection               |

Use **HTTP Streaming** when you need to deliver a large or incrementally generated payload as part of a normal request-response cycle.

