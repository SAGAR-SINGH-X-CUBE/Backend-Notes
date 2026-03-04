# Backend-Notes
Notes for Backend

# 📦 JavaScript Module Systems (CJS vs ESM)

---

## 1️⃣ CommonJS (CJS)

- Older module system
- Mainly used in traditional Node.js projects
- Loads modules synchronously (blocking)

### Export (CJS)

```js
// math.js
function add(a, b) {
  return a + b;
}

module.exports = add;
```

### Import (CJS)

```js
// app.js
const add = require('./math');

console.log(add(2, 3));
```

### How require() Works Internally

1. Stops execution
2. Loads the file
3. Executes the file
4. Returns module.exports
5. Continues execution

```js
console.log("Start");

const math = require('./math'); // blocking

console.log("End");
```

If the file is heavy → it blocks execution.

---

## 2️⃣ ES Modules (ESM)

- Modern JavaScript standard
- Used in React, Vite, Next.js, modern Node.js
- Loads modules asynchronously (non-blocking)

### Named Export

```js
// math.js
export function add(a, b) {
  return a + b;
}
```

### Named Import

```js
import { add } from './math.js';
```

### Default Export

```js
// math.js
export default function add(a, b) {
  return a + b;
}
```

### Default Import

```js
import add from './math.js';
```

---

## ⚡ CJS vs ESM Difference

| Feature        | CommonJS (CJS) | ES Modules (ESM) |
|---------------|----------------|------------------|
| Keyword       | require()     | import/export    |
| Loading Type  | Synchronous   | Asynchronous     |
| Blocking      | Yes           | No               |
| Usage         | Old Node.js   | Modern JS apps   |

---

# 🌍 What is CORS?

CORS = Cross-Origin Resource Sharing

It is a browser security policy that blocks cross-origin HTTP requests unless the server allows it.

---

## What is Origin?

Origin = Protocol + Domain + Port

Example:

Frontend:
http://localhost:5173

Backend:
http://localhost:3000

Different ports = Different origin  
So browser blocks the request.

---

# 🔥 How Proxy Solves CORS (During Development)

Instead of:

Frontend → Backend (blocked by browser)

We use:

Frontend → Proxy → Backend

Browser thinks request is same-origin.

---

## Simple Analogy

You are inside campus (5173).  
You want to talk to someone outside (3000).  

Security guard (browser) blocks you.

So you send your friend (proxy).  
Friend talks outside and brings response back.

Security thinks everything stayed inside.

Friend = Proxy

---

# ❓ Why Postman Never Shows CORS Error?

Because:

CORS is enforced by browsers.  
Postman is not a browser.

Postman sends request directly to server.  
No browser security → No CORS error.

---

# 🚀 Quick Revision Summary

- CJS → require() → Blocking
- ESM → import → Non-blocking
- CORS → Browser security rule
- Proxy → Avoids CORS in development
- Postman → No CORS because not a browser
