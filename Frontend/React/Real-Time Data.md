## Real-Time Data Handling in React

Handling real-time data correctly requires careful management of side effects outside the React render cycle. Connections (intervals, sockets) must be established on mount and torn down on unmount to prevent memory leaks.

The three primary strategies are **HTTP Polling**, **Server-Sent Events (SSE)**, and **WebSockets**.

### 1. HTTP Polling
The client actively requests data at fixed intervals. It is simple but least efficient (requests continue even when data is unchanged).

**Implementation Rules:**
- Use `setInterval` inside `useEffect`.
- Always clear the interval in the cleanup function.
- Add an active/inactive toggle to pause polling when needed.

```jsx
import React, { useState, useEffect } from 'react';

export default function PollingExample() {
  const [data, setData] = useState([]);
  const [isPolling, setIsPolling] = useState(true);

  useEffect(() => {
    let intervalId;

    const fetchData = async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=3');
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error("Polling error:", error);
      }
    };

    if (isPolling) {
      fetchData();
      intervalId = setInterval(fetchData, 5000);
    }

    return () => clearInterval(intervalId);
  }, [isPolling]);

  return (
    <div>
      <h2>Polling Dashboard</h2>
      <button onClick={() => setIsPolling(!isPolling)}>
        {isPolling ? 'Stop Polling' : 'Resume Polling'}
      </button>
      <ul>
        {data.map((item) => <li key={item.id}>{item.title}</li>)}
      </ul>
    </div>
  );
}
```

### 2. Server-Sent Events (SSE)
SSE provides a unidirectional, persistent HTTP connection. The server pushes updates to the client whenever new data is available.

**Implementation Rules:**
- Use the native `EventSource` API.
- Always use functional state updates (`setEvents(prev => [...prev, new])`) to avoid stale closures.
- Call `.close()` on the `EventSource` in the cleanup function.

```jsx
import React, { useState, useEffect } from 'react';

export default function SSEExample() {
  const [events, setEvents] = useState([]);
  const [status, setStatus] = useState('Connecting...');

  useEffect(() => {
    const eventSource = new EventSource('https://api.example.com/live-updates');

    eventSource.onopen = () => setStatus('Connected');
    
    eventSource.onmessage = (event) => {
      const newEventData = JSON.parse(event.data);
      setEvents((prevEvents) => {
        const updated = [...prevEvents, newEventData];
        return updated.slice(-10); // Keep only latest 10
      });
    };

    eventSource.onerror = () => {
      setStatus('Error');
      eventSource.close();
    };

    return () => eventSource.close();
  }, []);

  return (
    <div>
      <h2>Live Server Events (SSE)</h2>
      <p>Status: <strong>{status}</strong></p>
      <ul>
        {events.map((evt, index) => <li key={index}>{evt.message}</li>)}
      </ul>
    </div>
  );
}
```

### 3. WebSockets
WebSockets offer bidirectional (full-duplex) persistent communication over a single TCP connection.

**Implementation Rules:**
- Use the native `WebSocket` API (or `socket.io-client`).
- Store the socket instance in `useRef` to avoid re-renders.
- Use functional state updates for incoming messages.

```jsx
import React, { useState, useEffect, useRef } from 'react';

export default function WebSocketExample() {
  const [messages, setMessages] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [status, setStatus] = useState('Disconnected');
  const ws = useRef(null);

  useEffect(() => {
    ws.current = new WebSocket('wss://echo.websocket.events');

    ws.current.onopen = () => setStatus('Connected');
    ws.current.onclose = () => setStatus('Disconnected');
    
    ws.current.onmessage = (event) => {
      setMessages((prev) => [...prev, `Server: ${event.data}`]);
    };

    return () => {
      if (ws.current) ws.current.close();
    };
  }, []);

  const sendMessage = (e) => {
    e.preventDefault();
    if (ws.current?.readyState === WebSocket.OPEN && inputValue) {
      ws.current.send(inputValue);
      setMessages((prev) => [...prev, `You: ${inputValue}`]);
      setInputValue('');
    }
  };

  return (
    <div>
      <h2>Live Chat (WebSockets)</h2>
      <p>Status: <strong>{status}</strong></p>
      <form onSubmit={sendMessage}>
        <input 
          type="text" 
          value={inputValue} 
          onChange={(e) => setInputValue(e.target.value)} 
          placeholder="Type a message..."
          disabled={status !== 'Connected'}
        />
        <button type="submit" disabled={status !== 'Connected'}>Send</button>
      </form>
      <ul>
        {messages.map((msg, index) => <li key={index}>{msg}</li>)}
      </ul>
    </div>
  );
}
```