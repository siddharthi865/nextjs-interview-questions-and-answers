# Set 20

| #   | Question                                                                                                                                                                                |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement server-side WebSocket connections?](#question-1-how-do-you-implement-server-side-websocket-connections)                                                           |
| 2   | [How do you implement partial hydration for performance optimization?](#question-2-how-do-you-implement-partial-hydration-for-performance-optimization)                                 |
| 3   | [How do you optimize bundle size for large Next.js apps?](#question-3-how-do-you-optimize-bundle-size-for-large-nextjs-apps)                                                            |
| 4   | [How do you implement code splitting for heavy libraries like Chart.js or Three.js?](#question-4-how-do-you-implement-code-splitting-for-heavy-libraries-like-chartjs-or-threejs)       |
| 5   | [How do you implement background jobs triggered from API routes?](#question-5-how-do-you-implement-background-jobs-triggered-from-api-routes)                                           |
| 6   | [How do you implement caching strategies for large-scale ISR sites?](#question-6-how-do-you-implement-caching-strategies-for-large-scale-isr-sites)                                     |
| 7   | [How do you implement multi-language pages with SSR and SSG?](#question-7-how-do-you-implement-multi-language-pages-with-ssr-and-ssg)                                                   |
| 8   | [How do you implement AB testing for SSR pages?](#question-8-how-do-you-implement-ab-testing-for-ssr-pages)                                                                             |
| 9   | [How do you implement analytics at the edge for low-latency tracking?](#question-9-how-do-you-implement-analytics-at-the-edge-for-low-latency-tracking)                                 |
| 10  | [How do you implement server-side image generation dynamically?](#question-10-how-do-you-implement-server-side-image-generation-dynamically)                                            |
| 11  | [How do you integrate Next.js with Redis or Memcached for caching?](#question-11-how-do-you-integrate-nextjs-with-redis-or-memcached-for-caching)                                       |
| 12  | [How do you implement hybrid SSR + SSG for a content-heavy site?](#question-12-how-do-you-implement-hybrid-ssr--ssg-for-a-content-heavy-site)                                           |
| 13  | [How do you implement micro-frontends using Next.js?](#question-13-how-do-you-implement-micro-frontends-using-nextjs)                                                                   |
| 14  | [How do you debug memory leaks in SSR pages?](#question-14-how-do-you-debug-memory-leaks-in-ssr-pages)                                                                                  |
| 15  | [How do you monitor server-side performance in Next.js?](#question-15-how-do-you-monitor-server-side-performance-in-nextjs)                                                             |
| 16  | [How do you implement security headers in next.config.js?](#question-16-how-do-you-implement-security-headers-in-nextconfigjs)                                                          |
| 17  | [How do you implement Content Security Policy (CSP) in Next.js?](#question-17-how-do-you-implement-content-security-policy-csp-in-nextjs)                                               |
| 18  | [How do you handle high concurrency SSR pages efficiently?](#question-18-how-do-you-handle-high-concurrency-ssr-pages-efficiently)                                                      |
| 19  | [How do you implement server-side rate-limiting for API routes?](#question-19-how-do-you-implement-server-side-rate-limiting-for-api-routes)                                            |
| 20  | [What are the best practices for scaling a Next.js application for millions of users?](#question-20-what-are-the-best-practices-for-scaling-a-nextjs-application-for-millions-of-users) |

## Question 1. How do you implement server-side WebSocket connections?

## **Q: How do you implement server-side WebSocket connections?**

### **Short Answer (30 seconds):**

Next.js does **not** natively manage long-lived WebSocket servers because its request lifecycle is optimized for stateless HTTP requests, especially in serverless environments. In production, the recommended approach is to run a **separate WebSocket server** (using Socket.IO, `ws`, or similar) and let your Next.js application communicate with it via APIs, Redis, or shared services.

---

# **Detailed Explanation**

## 1. Why WebSockets are different

WebSockets provide **persistent, bidirectional communication** between the browser and the server.

Unlike normal HTTP requests:

- Connection stays open
- Server can push updates anytime
- Perfect for:
  - Chat applications
  - Live dashboards
  - Multiplayer games
  - Notifications
  - Collaborative editors
  - Stock prices

Next.js App Router is primarily designed for:

- Server Components
- Route Handlers
- Server Actions
- Streaming over HTTP

It is **not intended to host persistent WebSocket servers**.

> **Next.js 16 recommendation:** Run WebSockets as a separate service.

---

# 2. Production Architecture

```
Browser
   │
   ├────────────── HTTP ──────────────► Next.js App
   │                                      │
   │                                      │
   │                             Database/API
   │
   └──────────── WebSocket ─────────────► Socket Server
                                            │
                                            │
                                        Redis / PubSub
```

Advantages:

- Independent scaling
- No connection loss during deployments
- Works on Vercel/serverless
- Better reliability

---

# 3. Example using Socket.IO

### WebSocket server

```ts
import { Server } from "socket.io";

const io = new Server(3001, {
  cors: {
    origin: "http://localhost:3000",
  },
});

io.on("connection", (socket) => {
  console.log("User connected:", socket.id);

  socket.on("message", (msg) => {
    io.emit("message", msg);
  });

  socket.on("disconnect", () => {
    console.log("Disconnected");
  });
});
```

---

### Client Component

```tsx
"use client";

import { useEffect, useState } from "react";
import { io } from "socket.io-client";

const socket = io("http://localhost:3001");

export default function Chat() {
  const [messages, setMessages] = useState<string[]>([]);

  useEffect(() => {
    socket.on("message", (msg) => {
      setMessages((prev) => [...prev, msg]);
    });

    return () => {
      socket.off("message");
    };
  }, []);

  return (
    <>
      <button onClick={() => socket.emit("message", "Hello")}>Send</button>

      {messages.map((m, i) => (
        <p key={i}>{m}</p>
      ))}
    </>
  );
}
```

---

# 4. Using Route Handlers

Route Handlers (`app/api/.../route.ts`) are **not** designed to maintain long-lived WebSocket connections.

```ts
export async function GET() {
  return Response.json({
    message: "Use a dedicated WebSocket server",
  });
}
```

They should instead:

- Authenticate users
- Generate tokens
- Store chat history
- Trigger broadcasts through your WebSocket service

---

# 5. Authentication

A common production flow:

```
Login
   ↓
JWT issued
   ↓
Client connects

ws://socket-server?token=JWT

Socket verifies token

Connection accepted
```

Example:

```ts
io.use((socket, next) => {
  const token = socket.handshake.auth.token;

  // Verify JWT
  next();
});
```

Never trust client-provided user IDs without verification.

---

# 6. Scaling WebSockets

A single server works for development, but production often involves multiple instances.

```
Load Balancer
      │
 ┌────┴────┐
 │         │
Server1  Server2
 │         │
 └── Redis ──┘
```

With a Redis adapter:

```ts
io.to(roomId).emit("message", message);
```

Messages propagate across all WebSocket server instances.

---

# 7. Edge Runtime considerations

Edge Runtime currently does **not** support running traditional persistent WebSocket servers.

Use Edge for:

- Authentication
- Middleware
- Fast HTTP responses

Use a dedicated Node.js service for:

- WebSockets
- Socket.IO
- Real-time messaging

---

# 8. Performance considerations

- Reuse a single client socket instead of reconnecting on every render.
- Clean up listeners with `socket.off()` to prevent memory leaks.
- Use rooms/namespaces to limit broadcasts.
- Compress large payloads if appropriate.
- Implement heartbeat/ping mechanisms to detect stale connections.
- Handle automatic reconnection gracefully.
- Authenticate every connection.
- Use Redis or another pub/sub system for horizontal scaling.

---

# 9. Common pitfalls & solutions

| Pitfall                                                      | Solution                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| Hosting WebSocket server inside Next.js serverless functions | Use a dedicated Node.js WebSocket service  |
| Creating multiple client connections                         | Share a singleton socket instance          |
| Forgetting to remove listeners                               | Call `socket.off()` during cleanup         |
| Broadcasting to every client                                 | Use rooms or namespaces                    |
| No authentication                                            | Verify JWTs during the WebSocket handshake |
| Not planning for scaling                                     | Use Redis pub/sub or a Socket.IO adapter   |

---

# **Code (Production-ready structure)**

```tsx
// lib/socket.ts
"use client";

import { io } from "socket.io-client";

export const socket = io(process.env.NEXT_PUBLIC_SOCKET_URL!, {
  autoConnect: true,
  auth: {
    token: localStorage.getItem("token"),
  },
});
```

```tsx
// app/chat/page.tsx
"use client";

import { useEffect, useState } from "react";
import { socket } from "@/lib/socket";

export default function ChatPage() {
  const [messages, setMessages] = useState<string[]>([]);

  useEffect(() => {
    const onMessage = (msg: string) => {
      setMessages((prev) => [...prev, msg]);
    };

    socket.on("message", onMessage);

    return () => {
      socket.off("message", onMessage);
    };
  }, []);

  return (
    <button onClick={() => socket.emit("message", "Hello Next.js")}>
      Send Message
    </button>
  );
}
```

---

# **When to use**

Use server-side WebSocket connections when you need:

- Real-time chat
- Live notifications
- Multiplayer games
- Collaborative editing
- Live dashboards
- Presence indicators
- Financial or IoT data streams

---

# **Alternatives**

| Approach                                     | Best for                                                                                  |
| -------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Dedicated WebSocket server (recommended)** | Production-grade real-time applications                                                   |
| **Socket.IO**                                | Automatic reconnection, rooms, namespaces, and broad browser compatibility                |
| **`ws` library**                             | Lightweight, low-level WebSocket implementation with minimal overhead                     |
| **Server-Sent Events (SSE)**                 | One-way server-to-client streaming (e.g., live feeds or progress updates)                 |
| **Polling / Long Polling**                   | Simple applications with infrequent updates where persistent connections aren't necessary |

---

### **Next.js 16 Best Practice**

For production applications using the App Router, keep Next.js focused on rendering, routing, Server Components, and Server Actions. Host long-lived WebSocket connections in a separate Node.js service (or a managed real-time platform) and have your Next.js app communicate with it over HTTP or a pub/sub layer. This architecture scales better, works reliably with serverless deployments, and aligns with the intended design of Next.js.

**Relevant Next.js documentation:**

- App Router → Route Handlers
- App Router → Server Components
- App Router → Server Actions
- Edge Runtime vs. Node.js Runtime

## Question 2. How do you implement partial hydration for performance optimization?

## Question 3. How do you optimize bundle size for large Next.js apps?

## Question 4. How do you implement code splitting for heavy libraries like Chart.js or Three.js?

## Question 5. How do you implement background jobs triggered from API routes?

## Question 6. How do you implement caching strategies for large-scale ISR sites?

## Question 7. How do you implement multi-language pages with SSR and SSG?

## Question 8. How do you implement AB testing for SSR pages?

## Question 9. How do you implement analytics at the edge for low-latency tracking?

## Question 10. How do you implement server-side image generation dynamically?

## Question 11. How do you integrate Next.js with Redis or Memcached for caching?

## Question 12. How do you implement hybrid SSR + SSG for a content-heavy site?

## Question 13. How do you implement micro-frontends using Next.js?

## Question 14. How do you debug memory leaks in SSR pages?

## Question 15. How do you monitor server-side performance in Next.js?

## Question 16. How do you implement security headers in next.config.js?

## Question 17. How do you implement Content Security Policy (CSP) in Next.js?

## Question 18. How do you handle high concurrency SSR pages efficiently?

## Question 19. How do you implement server-side rate-limiting for API routes?

## Question 20. What are the best practices for scaling a Next.js application for millions of users?
