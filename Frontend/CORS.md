## 1. What is CORS?

**CORS** is a browser-based security mechanism that allows or restricts a web page from making requests to a domain different from the one that served the web page.

It exists to relax the **Same-Origin Policy (SOP)**. Under SOP, a browser only allows scripts on Page A to access data on Page B if both have the same **Origin**.

An **Origin** is defined by: **Protocol + Domain + Port**.

- `https://example.com:443`
- `http://example.com:443` -> **Different** (Different Protocol)
- `https://api.example.com` -> **Different** (Different Subdomain)

---

## 2. How CORS Works

CORS uses **HTTP Headers** to tell the browser if a cross-origin request is allowed.

### A. Simple Requests

For basic requests (like `GET` or `POST` with standard form data), the browser sends the request directly. The server must respond with the header:
`Access-Control-Allow-Origin: https://yourfrontend.com`

### B. Preflight Requests (The OPTIONS Method)

For "sensitive" requests (using `PUT`, `DELETE`, or custom headers like `Authorization`), the browser sends a **preflight** request first to check permissions.

1. **Browser sends `OPTIONS`:** "Hey, I'm from `App A`. Can I send a `DELETE` request with a `JSON` body?"
2. **Server responds:** "Yes, I allow `DELETE` and `Content-Type` headers for `App A`."
3. **Browser sends actual request:** Only after getting the "OK" does the browser send the real data.

---

## 3. Key CORS Headers

| Header                               | Description                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------ |
| **Access-Control-Allow-Origin**      | Specifies which domains can access the resource (e.g., `https://my-app.com` or `*`). |
| **Access-Control-Allow-Methods**     | List of allowed HTTP methods (`GET, POST, PUT, DELETE`).                             |
| **Access-Control-Allow-Headers**     | Specifies which custom headers can be used (e.g., `Content-Type, X-Custom-Header`).  |
| **Access-Control-Allow-Credentials** | Indicates if the request can include cookies or Authorization headers.               |

---

## 4. Implementation (Node.js/Express)

### Using the `cors` Middleware (Recommended)

```javascript
const cors = require("cors");

const corsOptions = {
  origin: "https://trusted-site.com",
  methods: ["GET", "POST", "DELETE"],
  allowedHeaders: ["Content-Type", "Authorization"],
};

app.use(cors(corsOptions));
```

### Manual Configuration

```javascript
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "https://trusted-site.com");
  res.header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE");
  res.header("Access-Control-Allow-Headers", "Content-Type, Authorization");

  // Handle Preflight
  if (req.method === "OPTIONS") {
    return res.sendStatus(200);
  }
  next();
});
```

---

## 5. Interview "Pro-Tips" & Gotchas

- **CORS is NOT a Server-to-Server security tool:** It is enforced by the **browser**. Tools like Postman, `curl`, or mobile apps do not care about CORS. To protect your server from them, use **Authentication (JWT/OAuth2)**.
- **The "Write-But-No-Read" Problem:** If a browser sends a `POST` request and the server doesn't have CORS configured, the server **might still execute the logic** (e.g., saving a user to the DB). The browser simply blocks the _response_ from being read by the JavaScript.
- **Wildcard `*` vs Credentials:** You **cannot** use `Access-Control-Allow-Origin: *` if `Access-Control-Allow-Credentials` is set to `true`. You must specify the exact origin.
