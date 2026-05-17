## Implementing WebSockets in React

WebSockets provide a persistent, full-duplex communication channel over a single Transmission Control Protocol (TCP) connection. Unlike standard HyperText Transfer Protocol (HTTP) requests, where the client must poll the server, WebSockets enable the server to push data instantly to the client (and vice versa) without repeatedly opening new connections.

The main challenge when using WebSockets in React is bridging the imperative WebSocket API with React’s declarative, state-driven lifecycle.

### The React WebSocket Strategy
A robust implementation manages three core concepts:

- **Lifecycle Management:** The connection opens when the component mounts and closes explicitly when it unmounts (via `useEffect` cleanup).
- **Instance Reference:** The WebSocket object is stored in a `useRef` to avoid triggering unwanted re-renders.
- **State Updates:** Event listeners form closures over initial state. Functional updates (e.g., `setMessages(prev => [...prev, new])`) prevent stale data issues.

### Implementation Example
The following component connects to the public echo server (`wss://echo.websocket.org`), sends messages, and displays echoed responses:

```jsx
import React, { useState, useEffect, useRef } from 'react';

export default function WebSocketEcho() {
  const [messages, setMessages] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [connectionStatus, setConnectionStatus] = useState('Connecting...');
  
  // Store the WebSocket instance in a ref
  const wsRef = useRef(null);

  useEffect(() => {
    const ws = new WebSocket('wss://echo.websocket.org');
    wsRef.current = ws;

    ws.onopen = () => setConnectionStatus('Connected ');
    
    ws.onmessage = (event) => {
      const incomingMessage = {
        id: Date.now(),
        text: event.data,
        sender: 'server'
      };
      setMessages((prevMessages) => [...prevMessages, incomingMessage]);
    };

    ws.onerror = (error) => {
      console.error('WebSocket Error:', error);
      setConnectionStatus('Error ');
    };

    ws.onclose = () => setConnectionStatus('Disconnected ');

    // Cleanup: close connection on unmount
    return () => {
      ws.close();
    };
  }, []); // Runs only once on mount

  const handleSendMessage = (e) => {
    e.preventDefault();
    if (!inputValue.trim()) return;

    if (wsRef.current && wsRef.current.readyState === WebSocket.OPEN) {
      wsRef.current.send(inputValue);

      const outgoingMessage = {
        id: Date.now() + 1,
        text: inputValue,
        sender: 'client'
      };
      setMessages((prevMessages) => [...prevMessages, outgoingMessage]);
      setInputValue('');
    }
  };

  return (
    <div style={{ maxWidth: '400px', margin: '20px auto', fontFamily: 'sans-serif' }}>
      <h2>Echo Server Chat</h2>
      <p style={{ fontWeight: 'bold' }}>Status: {connectionStatus}</p>

      <div style={{ 
        height: '300px', 
        overflowY: 'auto', 
        border: '1px solid #ccc', 
        padding: '10px',
        marginBottom: '10px',
        display: 'flex',
        flexDirection: 'column',
        gap: '8px'
      }}>
        {messages.length === 0 && <p style={{ color: '#888' }}>No messages yet...</p>}
        {messages.map((msg) => (
          <div 
            key={msg.id}
            style={{
              alignSelf: msg.sender === 'client' ? 'flex-end' : 'flex-start',
              backgroundColor: msg.sender === 'client' ? '#007bff' : '#e9ecef',
              color: msg.sender === 'client' ? '#fff' : '#000',
              padding: '8px 12px',
              borderRadius: '16px',
              maxWidth: '80%'
            }}
          >
            {msg.text}
          </div>
        ))}
      </div>

      <form onSubmit={handleSendMessage} style={{ display: 'flex', gap: '8px' }}>
        <input
          type="text"
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="Type a message..."
          disabled={connectionStatus !== 'Connected '}
          style={{ flexGrow: 1, padding: '8px' }}
        />
        <button 
          type="submit" 
          disabled={connectionStatus !== 'Connected ' || !inputValue.trim()}
          style={{ padding: '8px 16px', cursor: 'pointer' }}
        >
          Send
        </button>
      </form>
    </div>
  );
}
```

### Production Considerations
The native WebSocket API requires additional handling for real-world reliability:

- **Auto-Reconnection:** Implement a backoff algorithm or use a library such as ReconnectingWebSocket, as native connections do not automatically recover from network drops.
- **Heartbeats (Ping/Pong):** Send periodic “ping” messages (e.g., every 30 seconds) to prevent proxies or load balancers from closing idle connections.
- **Component Unmounting & Fast Refresh:** Always close the socket in the `useEffect` cleanup. Without this, Strict Mode or Fast Refresh can spawn multiple connections and cause memory leaks.