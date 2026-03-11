<div align="center">

<img src="https://img.shields.io/badge/BeautyKit-Backend_API-ff69b4?style=for-the-badge&logo=node.js&logoColor=white" alt="BeautyKit Backend" />

# 🌸 BeautyKit — Backend API

### The intelligent REST API powering skin tone analysis & personalized beauty recommendations

[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![Express](https://img.shields.io/badge/Express-4.18-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com)
[![JWT](https://img.shields.io/badge/JWT-Auth-orange?style=flat-square&logo=jsonwebtokens&logoColor=white)](https://jwt.io)
[![bcrypt](https://img.shields.io/badge/bcryptjs-Secure-blue?style=flat-square)](https://www.npmjs.com/package/bcryptjs)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://github.com/beautykit/pulls)
[![Stars](https://img.shields.io/github/stars/beautykit/BeautyKit?style=flat-square&color=gold)](https://github.com/beautykit/BeautyKit/stargazers)

---

> **BeautyKit's backend** is a blazing-fast Node.js + Express REST API that accepts raw RGB pixel data from a user's skin, runs a proprietary undertone & depth detection algorithm, and returns a full, deeply curated set of personalized beauty recommendations — covering jewelry, clothing, lipstick, blush, eyeshadow, and hair — all protected behind a complete JWT authentication system.

---

[🚀 Quick Start](#-quick-start) • [📐 Architecture](#-architecture-overview) • [🔐 Auth System](#-authentication-system-deep-dive) • [🎨 Skin Analysis](#-skin-analysis-engine-deep-dive) • [💄 Recommendations](#-beauty-recommendations-engine) • [📡 API Reference](#-api-reference) • [🔮 Roadmap](#-roadmap--future-plans) • [🤝 Contributing](#-contributing)

</div>

---

## 📋 Table of Contents

1. [Project Overview](#-project-overview)
2. [Tech Stack](#-tech-stack)
3. [Project Structure](#-project-structure)
4. [Quick Start](#-quick-start)
5. [Environment & Configuration](#-environment--configuration)
6. [Architecture Overview](#-architecture-overview)
7. [Authentication System — Deep Dive](#-authentication-system-deep-dive)
8. [In-Memory Database — Deep Dive](#-in-memory-database-deep-dive)
9. [Auth Middleware — Deep Dive](#-auth-middleware-deep-dive)
10. [Skin Analysis Engine — Deep Dive](#-skin-analysis-engine-deep-dive)
11. [Beauty Recommendations Engine](#-beauty-recommendations-engine)
12. [Profile Management — Deep Dive](#-profile-management-deep-dive)
13. [API Reference](#-api-reference)
14. [Data Models & Schemas](#-data-models--schemas)
15. [Error Handling Strategy](#-error-handling-strategy)
16. [Security Considerations](#-security-considerations)
17. [Performance Notes](#-performance-notes)
18. [Roadmap & Future Plans](#-roadmap--future-plans)
19. [Contributing](#-contributing)
20. [Code of Conduct](#-code-of-conduct)
21. [License](#-license)

---

## 🌟 Project Overview

BeautyKit's backend is the brain of the entire application. While the frontend captures and renders, **the backend thinks**. It is responsible for:

- **User Identity** — Registering and authenticating users with industry-standard security (bcrypt + JWT)
- **Pixel Intelligence** — Taking a single average RGB value from a user's skin tone photo and running it through a multi-factor color science algorithm to produce an undertone classification (`warm`, `cool`, or `neutral`) and a depth classification (`light`, `light-medium`, `medium`, `medium-deep`, or `deep`)
- **Recommendation Generation** — Mapping that classified skin tone to an enormous, hand-curated dataset of beauty recommendations across 7 distinct categories
- **Profile Persistence** — Allowing users to name, save, and manage multiple "beauty profiles" (e.g., one per person in a household, or seasonal looks)
- **Stateless REST Design** — Every request is independently authorized through a Bearer token. No session cookies, no server-side sessions

This server is purposefully written as a **single-file Express application** (`index.js`) to keep it approachable for contributors of all levels. It is production-upgradeable — all the comments and structure guide you toward swapping the in-memory store for a real database when you're ready.

---

## 🛠 Tech Stack

| Technology | Version | Role |
|---|---|---|
| **Node.js** | 18+ | JavaScript runtime |
| **Express** | 4.18.2 | HTTP server & routing framework |
| **bcryptjs** | 2.4.3 | Password hashing (pure JS, no native bindings) |
| **jsonwebtoken** | 9.0.2 | JWT signing & verification |
| **uuid** | 9.0.0 | RFC 4122 UUID v4 generation |
| **cors** | 2.8.5 | Cross-Origin Resource Sharing headers |
| **nodemon** | 3.0.1 *(dev)* | Auto-restart during development |

### Why these choices?

- **`bcryptjs` over `bcrypt`**: The pure-JavaScript `bcryptjs` package requires no native compilation, making it universally installable on any OS/architecture without a C++ build toolchain. Performance difference at scale is negligible for auth endpoints.
- **`jsonwebtoken`**: The de-facto standard JWT library for Node.js. Maintained by Auth0, battle-tested in millions of applications.
- **`uuid v4`**: Cryptographically random UUIDs prevent enumeration attacks on user and profile IDs.
- **`express` 4.x**: Mature, minimal, and unopinionated. Perfect for a focused API server.

---

## 📁 Project Structure

```
BeautyKit/backend/
│
├── index.js              ← The entire API server (single-file architecture)
├── package.json          ← Dependencies & npm scripts
├── package-lock.json     ← Locked dependency tree
├── .gitignore            ← Standard Node .gitignore
├── LICENSE               ← MIT License
└── README.md             ← You are here
```

The entire API logic lives in `index.js`. This is an intentional design decision for:
- **Readability** — A new contributor can read one file and understand the entire system
- **Simplicity** — No module resolution complexity, no barrel exports to trace
- **Portability** — Trivial to copy, deploy, or refactor into a modular structure

---

## 🚀 Quick Start

### Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/) v18 or higher
- npm v9 or higher (comes with Node.js)

### 1. Clone the repository

```sh
git clone https://github.com/beautykit/BeautyKit.git
cd BeautyKit/backend
```

### 2. Install dependencies

```sh
npm install
```

### 3. Start the development server

```sh
npm run dev
```

This uses `nodemon` to automatically restart the server whenever you save changes to `index.js`.

### 4. Start the production server

```sh
npm start
```

### 5. Verify the server is running

```sh
curl http://localhost:5000/api/health
```

Expected response:

```json
{
  "status": "ok",
  "message": "BeauKit API v2 Running"
}
```

The API is now live at `http://localhost:5000`. 🎉

---

## ⚙️ Environment & Configuration

Currently, the server uses hardcoded constants at the top of `index.js`:

```js
const PORT = 5000;
const JWT_SECRET = 'beaukit-secret-key-2024';
```

> ⚠️ **Important for production**: The `JWT_SECRET` **must** be replaced with a long, randomly generated secret stored in an environment variable. Never commit a real secret to version control.

### Recommended production setup

Install `dotenv`:

```sh
npm install dotenv
```

Create a `.env` file (already gitignored):

```env
PORT=5000
JWT_SECRET=your-super-long-random-secret-here-at-least-256-bits
```

Then update `index.js`:

```js
require('dotenv').config();
const PORT = process.env.PORT || 5000;
const JWT_SECRET = process.env.JWT_SECRET;
```

---

## 📐 Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT (React)                       │
│                                                             │
│  sends: { Authorization: "Bearer <token>" }                 │
│  sends: { r, g, b }  ←─ avg pixel from canvas              │
└─────────────────────┬───────────────────────────────────────┘
                      │  HTTP/REST
                      ▼
┌─────────────────────────────────────────────────────────────┐
│                   EXPRESS SERVER (:5000)                    │
│                                                             │
│  ┌──────────────┐  ┌───────────────┐  ┌─────────────────┐  │
│  │  CORS        │  │ express.json  │  │  authMiddleware  │  │
│  │  Middleware  │  │ Body Parser   │  │  (JWT Verify)    │  │
│  └──────────────┘  └───────────────┘  └─────────────────┘  │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                    ROUTE HANDLERS                    │   │
│  │                                                      │   │
│  │  POST /api/auth/signup   → register new user        │   │
│  │  POST /api/auth/signin   → login, return JWT        │   │
│  │  GET  /api/auth/me       → return current user      │   │
│  │  GET  /api/profiles      → list user's profiles     │   │
│  │  POST /api/profiles      → save a new profile       │   │
│  │  DELETE /api/profiles/:id→ delete a profile         │   │
│  │  POST /api/analyze-pixels→ analyze RGB skin data    │   │
│  │  GET  /api/health        → server health check      │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │               ANALYSIS ENGINE                        │   │
│  │                                                      │   │
│  │  analyzeSkinTone(r,g,b) → { undertone, depth }      │   │
│  │  getRecommendations(undertone, depth) → { ... }     │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │            IN-MEMORY DATABASE (db object)            │   │
│  │                                                      │   │
│  │  db.users   → { [email]: userObject }               │   │
│  │  db.sessions→ (reserved for future use)             │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

The flow for a skin analysis request looks like this:

```
Client Canvas
    │
    ▼
Extract avg pixel (R, G, B) from skin region
    │
    ▼
POST /api/analyze-pixels  { r, g, b }
    │
    ▼
analyzeSkinTone(r, g, b)
    │
    ├─► Calculate warmth  = (R - B) / 255
    ├─► Calculate greenBal = G / (R + G + B + 1)
    ├─► Determine undertone → warm | cool | neutral
    ├─► Calculate brightness = (R + G + B) / 3
    └─► Determine depth → light | light-medium | medium | medium-deep | deep
    │
    ▼
getRecommendations(undertone, depth)
    │
    └─► Lookup beauty data for undertone category
    │
    ▼
Map { depth + undertone } → human-readable tone name
    │
    ▼
Return full JSON response to client
```

---

## 🔐 Authentication System — Deep Dive

The auth system is built on two industry standards: **bcryptjs** for password storage and **JSON Web Tokens** for stateless session management.

### Sign Up — `POST /api/auth/signup`

```js
app.post('/api/auth/signup', async (req, res) => {
  try {
    const { name, email, password } = req.body;

    // 1. Validate all required fields are present
    if (!name || !email || !password)
      return res.status(400).json({ error: 'All fields required' });

    // 2. Check for duplicate email (O(1) lookup via object key)
    if (db.users[email])
      return res.status(409).json({ error: 'Email already registered' });

    // 3. Hash the password — bcrypt with cost factor 10
    //    Cost factor 10 means 2^10 = 1024 hashing rounds
    //    This makes brute-forcing computationally expensive
    const passwordHash = await bcrypt.hash(password, 10);

    // 4. Generate a cryptographically random UUID for the user
    const id = uuidv4();

    // 5. Store user in the in-memory db
    db.users[email] = {
      id,
      name,
      email,
      passwordHash,      // ← NEVER store plaintext passwords
      profiles: [],      // ← Each user starts with an empty profiles array
      createdAt: new Date().toISOString()
    };

    // 6. Sign a JWT containing the userId and email
    //    Expires in 7 days — user stays logged in for a week
    const token = jwt.sign({ userId: id, email }, JWT_SECRET, { expiresIn: '7d' });

    // 7. Return the token and safe user data (never return passwordHash)
    res.json({ token, user: { id, name, email } });

  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

**Key decisions explained:**

| Decision | Reason |
|---|---|
| `bcrypt.hash(password, 10)` | Cost factor 10 is the OWASP-recommended minimum. Each +1 doubles the work. |
| `uuidv4()` for IDs | Prevents sequential ID enumeration attacks that integer IDs expose. |
| Keying `db.users` by email | O(1) duplicate check and O(1) user lookup by email on login. |
| `expiresIn: '7d'` | Balances security (token expires) with UX (user doesn't log in daily). |
| Returning `{ id, name, email }` | Strips `passwordHash`, `createdAt`, and internal data from the response. |

---

### Sign In — `POST /api/auth/signin`

```js
app.post('/api/auth/signin', async (req, res) => {
  try {
    const { email, password } = req.body;

    // 1. Lookup user — O(1)
    const user = db.users[email];

    // 2. Intentionally vague error: don't reveal whether email exists
    //    This prevents user enumeration attacks
    if (!user)
      return res.status(401).json({ error: 'Invalid email or password' });

    // 3. bcrypt.compare is timing-safe — prevents timing-based attacks
    //    It hashes the candidate password and compares hash-to-hash
    const valid = await bcrypt.compare(password, user.passwordHash);

    if (!valid)
      return res.status(401).json({ error: 'Invalid email or password' });

    // 4. Issue a fresh JWT on every successful login
    const token = jwt.sign(
      { userId: user.id, email },
      JWT_SECRET,
      { expiresIn: '7d' }
    );

    res.json({ token, user: { id: user.id, name: user.name, email } });

  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

**Security highlight:** The error message `"Invalid email or password"` is intentionally identical whether the email doesn't exist OR the password is wrong. This is called **credential ambiguity** and it prevents attackers from probing which emails are registered.

---

### Get Current User — `GET /api/auth/me`

```js
app.get('/api/auth/me', authMiddleware, (req, res) => {
  const user = db.users[req.userEmail];
  if (!user) return res.status(404).json({ error: 'User not found' });
  res.json({
    id: user.id,
    name: user.name,
    email: user.email,
    profileCount: user.profiles.length  // ← handy for UI badges
  });
});
```

This route is used by the frontend on app load to re-hydrate the authenticated user's state from a stored token, avoiding a forced re-login after a page refresh. The `authMiddleware` dependency is what gates it.

---

## 🗄 In-Memory Database — Deep Dive

```js
const db = {
  users: {},    // { [email]: { id, name, email, passwordHash, profiles[], createdAt } }
  sessions: {}  // Reserved — not yet used
};
```

The database is a plain JavaScript object held in the Node.js process memory. This is an intentional tradeoff:

**Advantages:**
- Zero setup — no database server to install or configure
- Zero latency — all reads/writes are direct memory access
- Zero serialization — JavaScript objects don't need to be parsed or unparsed
- Perfect for development, demos, and prototypes

**Disadvantages:**
- **Data is lost on server restart** — all users and profiles disappear
- **Not shared** — if you run multiple Node.js instances (e.g. on a load balancer), each has its own isolated `db` object
- **Memory-bound** — at scale, millions of users would exhaust RAM

### How to upgrade to a real database

The codebase is structured so that swapping the `db` object for real database calls is a matter of replacing the contents of each route handler. The route interfaces remain identical.

**For MongoDB (with Mongoose):**

```js
// Replace: db.users[email] = { ... }
// With:
await User.create({ id, name, email, passwordHash, profiles: [] });

// Replace: const user = db.users[email]
// With:
const user = await User.findOne({ email });
```

**For PostgreSQL (with Prisma):**

```js
// Replace: db.users[email] = { ... }
// With:
await prisma.user.create({ data: { id, name, email, passwordHash } });

// Replace: const user = db.users[email]
// With:
const user = await prisma.user.findUnique({ where: { email } });
```

The `profiles` array currently lives nested inside each user object. In a relational database, you would normalize this into a separate `profiles` table with a foreign key `userId`.

---

## 🔒 Auth Middleware — Deep Dive

```js
function authMiddleware(req, res, next) {
  // 1. Extract the token from the Authorization header
  //    Expected format: "Authorization: Bearer eyJhbGci..."
  const token = req.headers.authorization?.split(' ')[1];

  // 2. If no token is present at all, reject immediately
  if (!token)
    return res.status(401).json({ error: 'No token provided' });

  try {
    // 3. jwt.verify does TWO things simultaneously:
    //    a) Verifies the token's signature against JWT_SECRET
    //    b) Checks that the token hasn't expired (expiresIn: '7d')
    //    If either check fails, it throws an error
    const decoded = jwt.verify(token, JWT_SECRET);

    // 4. Attach the decoded payload to the request object
    //    so downstream route handlers can read the identity
    req.userId = decoded.userId;
    req.userEmail = decoded.email;

    // 5. Call next() to pass control to the route handler
    next();

  } catch {
    // 6. Any verification failure → 401 Unauthorized
    //    This catches: expired tokens, tampered tokens, wrong secret
    res.status(401).json({ error: 'Invalid token' });
  }
}
```

This middleware is passed as the **second argument** to any route that requires authentication:

```js
app.get('/api/profiles', authMiddleware, (req, res) => { ... });
//                       ↑ runs first, then the handler
```

**How JWT verification works under the hood:**

A JWT has three parts separated by dots: `header.payload.signature`

1. **Header** — contains the algorithm (e.g. `HS256`)
2. **Payload** — contains the claims (`userId`, `email`, `exp`)
3. **Signature** — `HMACSHA256(base64(header) + "." + base64(payload), JWT_SECRET)`

When `jwt.verify()` runs, it re-computes the signature from the header and payload using `JWT_SECRET` and compares it to the signature in the token. If they don't match, the token was tampered with. If `exp` (expiry timestamp) is in the past, the token is expired. Both scenarios throw an error caught by the `catch` block.

---

## 🎨 Skin Analysis Engine — Deep Dive

This is the intellectual core of BeautyKit. Two pure functions — `analyzeSkinTone` and `getRecommendations` — form the complete analysis pipeline.

### `analyzeSkinTone(r, g, b)` — The Color Science

```js
function analyzeSkinTone(r, g, b) {
  // ── Step 1: Determine Undertone ──────────────────────────────

  // "Warmth" measures the red-to-blue ratio.
  // Warm skin tones have significantly more red than blue.
  // Range: -1.0 (purely blue) to +1.0 (purely red)
  const warmth = (r - b) / 255;

  // "greenBal" (green balance) measures the proportion of green
  // in the total color. Warm skin tends to be low in green;
  // cool/neutral skin has a more balanced green contribution.
  const greenBal = g / (r + g + b + 1);
  // The +1 prevents division by zero for pure black (0,0,0)

  // Undertone decision tree:
  //   warmth > 0.15 AND greenBal < 0.36  →  WARM
  //     (strong red presence, limited green channel)
  //   warmth < 0.05                       →  COOL
  //     (red and blue are nearly equal or blue dominates)
  //   everything else                     →  NEUTRAL
  //     (balanced across channels)
  let undertone =
    warmth > 0.15 && greenBal < 0.36 ? 'warm' :
    warmth < 0.05                     ? 'cool' :
                                        'neutral';

  // ── Step 2: Determine Depth ──────────────────────────────────

  // "Brightness" is the arithmetic mean of all three channels.
  // This is a simplified luminance calculation (not perceptual,
  // but consistent and fast for classification purposes).
  const brightness = (r + g + b) / 3;

  // Depth classification by brightness ranges:
  // > 200   → light       (fair/porcelain skin)
  // > 165   → light-medium (light beige/ivory)
  // > 125   → medium      (golden beige/tan)
  // > 85    → medium-deep (warm bronze/tan)
  // ≤ 85    → deep        (deep brown/ebony)
  let depth =
    brightness > 200 ? 'light'        :
    brightness > 165 ? 'light-medium' :
    brightness > 125 ? 'medium'       :
    brightness > 85  ? 'medium-deep'  :
                       'deep';

  return { undertone, depth, brightness };
}
```

**Why this formula works:**

Human skin tone, regardless of ethnicity, varies primarily along two axes:
1. **Undertone** — the hue bias of melanin and hemoglobin (warm = yellow/orange bias; cool = pink/blue bias; neutral = balanced)
2. **Depth** — the concentration of melanin controlling lightness/darkness

The `warmth` formula `(R - B) / 255` exploits the fact that warm undertones are physically caused by carotenoids and eumelanin (both of which skew toward red-orange), while cool undertones are influenced by hemoglobin visibility (pink) and pheomelanin (blue-pink). By computing the normalized difference between the red and blue channels, we get a reliable warmth signal across the full depth spectrum.

The `greenBal` secondary check prevents pure reds (like sunburns or flush) from being misclassified as warm undertones — a truly warm skin tone has a bounded green channel that sits in a characteristic range.

**Mapping `{depth}-{undertone}` to human-readable tone names:**

```js
const toneNames = {
  'light-warm':        'Porcelain Warm',
  'light-medium-warm': 'Fair Warm',
  'medium-warm':       'Golden Beige',
  'medium-deep-warm':  'Warm Bronze',
  'deep-warm':         'Deep Mahogany',
  'light-cool':        'Porcelain Cool',
  'light-medium-cool': 'Fair Cool',
  'medium-cool':       'Cool Beige',
  'medium-deep-cool':  'Cool Tan',
  'deep-cool':         'Deep Ebony',
  'light-neutral':     'Porcelain',
  'light-medium-neutral': 'Fair Neutral',
  'medium-neutral':    'Natural Beige',
  'medium-deep-neutral': 'Neutral Tan',
  'deep-neutral':      'Rich Espresso'
};
```

This produces 15 distinct, inclusive, and cosmetically meaningful skin tone names that map onto the 3 undertones × 5 depths matrix.

---

## 💄 Beauty Recommendations Engine

The `getRecommendations(undertone, depth)` function returns a deeply nested object structured as:

```
recommendations/
├── jewelry/
│   ├── metals[]       → { name, hex, rating (1-5), note }
│   ├── styles[]       → string[]
│   ├── gemstones[]    → string[]
│   └── avoid[]        → string[]
│
├── clothing/
│   ├── colors[]       → { name, hex, category }
│   ├── styles[]       → string[]
│   ├── fabrics[]      → string[]
│   ├── patterns[]     → string[]
│   └── avoid[]        → string[]
│
├── lipstick[]         → { name, hex, finish, vibe }
├── blush[]            → { name, hex, finish }
├── eyeshadow[]        → { name, hex }
│
└── hair/
    ├── colors[]       → { name, hex, level }
    ├── styles[]       → string[]
    ├── treatments[]   → string[]
    └── avoid          → string
```

### Recommendation logic by undertone

#### 🌅 Warm Undertones
Users with warm undertones have yellow, golden, or peachy hues in their skin. The recommendations reflect this by leaning into earth tones, gold metals, and analogous warm colors.

- **Jewelry metals:** Yellow gold (★★★★★), Rose gold (★★★★), Copper, Bronze, Brass — all metals with a golden-orange warmth
- **Avoid:** Silver, White Gold, Platinum (cool metals create a stark contrast that can look ashy)
- **Clothing:** Terracotta, Olive, Rust, Camel, Coral, Mustard — the earth and nature palette
- **Lipstick:** Warm Coral, Peachy Nude, Terracotta Red, Cinnamon — rich and golden finishes
- **Hair:** Golden Blonde, Honey Brown, Warm Auburn, Copper Red — warm-spectrum hair pigments

#### ❄️ Cool Undertones
Users with cool undertones have pink, rosy, or bluish hues. The recommendations use metals and colors from the blue-violet spectrum.

- **Jewelry metals:** Sterling Silver (★★★★★), White Gold (★★★★★), Platinum, Rhodium — bright cool metals
- **Avoid:** Yellow Gold, Copper, Bronze (warm metals can make cool skin look muddy)
- **Clothing:** Icy Lavender, Navy, Emerald Green, True White, Royal Blue, Plum
- **Lipstick:** Cool Berry, True Red, Dusty Rose, Deep Plum, Burgundy
- **Hair:** Ash Blonde, Platinum, Blue-Black, Ice Blonde, Violet Brown

#### 🌸 Neutral Undertones
Users with neutral undertones have a balance of warm and cool. They are the most versatile — nearly any color works, so recommendations lean into that freedom.

- **Jewelry metals:** Rose Gold (★★★★★), Mixed Metal (★★★★★) — neutral tones can wear anything
- **Avoid:** Nothing! This is explicitly called out in the data: `"Nothing — neutral tones are the most versatile!"`
- **Clothing:** Dusty Rose, Sage Green, Soft Navy, Teal, Lilac — soft and balanced palette
- **Hair:** Medium Brown, Dark Blonde, Chestnut, Toffee — balanced tone hair colors

### The `POST /api/analyze-pixels` handler in full

```js
app.post('/api/analyze-pixels', (req, res) => {
  try {
    const { r, g, b } = req.body;

    // Validate that RGB values were provided
    if (r === undefined || g === undefined || b === undefined)
      return res.status(400).json({ error: 'RGB values required' });

    // Run the two-step analysis
    const { undertone, depth } = analyzeSkinTone(r, g, b);
    const recommendations = getRecommendations(undertone, depth);

    // Build a human-readable tone name from the lookup table
    const toneName = toneNames[`${depth}-${undertone}`] || `${depth} ${undertone}`;

    // Build the hex string from the raw RGB values
    const hex =
      '#' +
      r.toString(16).padStart(2, '0') +
      g.toString(16).padStart(2, '0') +
      b.toString(16).padStart(2, '0');

    // Return the full analysis payload
    res.json({
      skinTone: {
        name: toneName,
        undertone,
        depth,
        rgb: { r, g, b },
        hex
      },
      recommendations
    });

  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

**Note:** This route is intentionally NOT protected by `authMiddleware`. Skin analysis is a public feature — guests can analyze their skin tone without creating an account. Only saving the result as a named profile requires authentication.

---

## 🗂 Profile Management — Deep Dive

Profiles are the "memory" of BeautyKit. A user can run an analysis, name the result (e.g. "My Summer Look" or "Mom's Profile"), and save it for later retrieval. Each profile stores a snapshot of the skin tone data and full recommendations at the time of saving.

### Save a Profile — `POST /api/profiles`

```js
app.post('/api/profiles', authMiddleware, (req, res) => {
  try {
    const user = db.users[req.userEmail];
    if (!user) return res.status(404).json({ error: 'User not found' });

    const { profileName, skinTone, recommendations, avatar } = req.body;

    // profileName is the only required field —
    // a user must intentionally name their profile
    if (!profileName)
      return res.status(400).json({ error: 'Profile name required' });

    const profile = {
      id: uuidv4(),             // unique ID for delete operations
      profileName,              // e.g. "Golden Hour Look"
      skinTone,                 // the full skinTone object from analyze-pixels
      recommendations,          // the full recommendations snapshot
      avatar: avatar || null,   // optional base64 image from the canvas
      createdAt: new Date().toISOString()
    };

    // Push into the user's profiles array (nested inside the user object)
    user.profiles.push(profile);

    // Return the newly created profile with its generated ID
    res.json(profile);

  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Get All Profiles — `GET /api/profiles`

```js
app.get('/api/profiles', authMiddleware, (req, res) => {
  const user = db.users[req.userEmail];
  if (!user) return res.status(404).json({ error: 'User not found' });

  // Return the entire profiles array for this user
  // The array preserves insertion order (newest at end)
  res.json(user.profiles);
});
```

### Delete a Profile — `DELETE /api/profiles/:id`

```js
app.delete('/api/profiles/:id', authMiddleware, (req, res) => {
  const user = db.users[req.userEmail];
  if (!user) return res.status(404).json({ error: 'User not found' });

  // Find the profile by its UUID
  const idx = user.profiles.findIndex(p => p.id === req.params.id);

  if (idx === -1)
    return res.status(404).json({ error: 'Profile not found' });

  // splice mutates the array in place — O(n) but profiles arrays are small
  user.profiles.splice(idx, 1);

  res.json({ success: true });
});
```

**Security note:** Because all profile operations are gated by `authMiddleware`, a user can only access and modify profiles belonging to their own account. The `req.userEmail` is extracted from the verified JWT — it cannot be spoofed by the client.

---

## 📡 API Reference

### Base URL
```
http://localhost:5000
```

### Authentication
All protected routes require an `Authorization` header:
```
Authorization: Bearer <your_jwt_token>
```

---

### 🔓 Public Endpoints

#### `GET /api/health`
Health check — verify the server is running.

| | |
|---|---|
| **Auth required** | No |
| **Body** | None |

**Response `200`:**
```json
{
  "status": "ok",
  "message": "BeauKit API v2 Running"
}
```

---

#### `POST /api/auth/signup`
Register a new user account.

| | |
|---|---|
| **Auth required** | No |
| **Content-Type** | `application/json` |

**Request body:**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "mySecurePassword123"
}
```

**Response `200`:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "a3f8c2d1-...",
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

**Error responses:**
| Status | Error |
|---|---|
| `400` | `All fields required` |
| `409` | `Email already registered` |
| `500` | Server error message |

---

#### `POST /api/auth/signin`
Authenticate an existing user.

| | |
|---|---|
| **Auth required** | No |
| **Content-Type** | `application/json` |

**Request body:**
```json
{
  "email": "jane@example.com",
  "password": "mySecurePassword123"
}
```

**Response `200`:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "a3f8c2d1-...",
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

**Error responses:**
| Status | Error |
|---|---|
| `401` | `Invalid email or password` |
| `500` | Server error message |

---

#### `POST /api/analyze-pixels`
Analyze a skin tone from average RGB pixel values and receive beauty recommendations.

| | |
|---|---|
| **Auth required** | No |
| **Content-Type** | `application/json` |

**Request body:**
```json
{
  "r": 198,
  "g": 158,
  "b": 120
}
```

**Response `200`:**
```json
{
  "skinTone": {
    "name": "Golden Beige",
    "undertone": "warm",
    "depth": "medium",
    "rgb": { "r": 198, "g": 158, "b": 120 },
    "hex": "#c69e78"
  },
  "recommendations": {
    "jewelry": { "metals": [...], "styles": [...], "gemstones": [...], "avoid": [...] },
    "clothing": { "colors": [...], "styles": [...], "fabrics": [...], "patterns": [...], "avoid": [...] },
    "lipstick": [...],
    "blush": [...],
    "eyeshadow": [...],
    "hair": { "colors": [...], "styles": [...], "treatments": [...], "avoid": "..." }
  }
}
```

**Error responses:**
| Status | Error |
|---|---|
| `400` | `RGB values required` |
| `500` | Server error message |

---

### 🔐 Protected Endpoints

All routes below require `Authorization: Bearer <token>`.

---

#### `GET /api/auth/me`
Get the currently authenticated user's info.

**Response `200`:**
```json
{
  "id": "a3f8c2d1-...",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "profileCount": 3
}
```

---

#### `GET /api/profiles`
Get all saved beauty profiles for the current user.

**Response `200`:**
```json
[
  {
    "id": "b7e2f1a0-...",
    "profileName": "My Summer Look",
    "skinTone": { ... },
    "recommendations": { ... },
    "avatar": "data:image/png;base64,...",
    "createdAt": "2024-07-15T10:30:00.000Z"
  }
]
```

---

#### `POST /api/profiles`
Save a new beauty profile.

**Request body:**
```json
{
  "profileName": "My Summer Look",
  "skinTone": { "name": "Golden Beige", "undertone": "warm", "depth": "medium", ... },
  "recommendations": { ... },
  "avatar": "data:image/png;base64,..."
}
```

**Response `200`:** The newly created profile object with its generated `id` and `createdAt`.

**Error responses:**
| Status | Error |
|---|---|
| `400` | `Profile name required` |
| `404` | `User not found` |

---

#### `DELETE /api/profiles/:id`
Delete a saved profile by its UUID.

**URL parameter:** `:id` — the UUID of the profile to delete

**Response `200`:**
```json
{ "success": true }
```

**Error responses:**
| Status | Error |
|---|---|
| `404` | `Profile not found` |
| `404` | `User not found` |

---

## 📦 Data Models & Schemas

### User Object (internal — never fully exposed via API)

```ts
{
  id: string;             // UUID v4
  name: string;           // Display name
  email: string;          // Unique identifier & lookup key
  passwordHash: string;   // bcrypt hash — NEVER returned in API responses
  profiles: Profile[];    // Array of saved beauty profiles
  createdAt: string;      // ISO 8601 timestamp
}
```

### Profile Object

```ts
{
  id: string;             // UUID v4
  profileName: string;    // User-defined name (e.g. "Mom's Profile")
  skinTone: {
    name: string;         // e.g. "Golden Beige"
    undertone: 'warm' | 'cool' | 'neutral';
    depth: 'light' | 'light-medium' | 'medium' | 'medium-deep' | 'deep';
    rgb: { r: number; g: number; b: number };
    hex: string;          // e.g. "#c69e78"
  };
  recommendations: RecommendationSet;
  avatar: string | null;  // base64 data URL or null
  createdAt: string;      // ISO 8601 timestamp
}
```

### JWT Payload

```ts
{
  userId: string;   // UUID v4 — same as User.id
  email: string;    // User's email address
  iat: number;      // Issued-at timestamp (auto-added by jsonwebtoken)
  exp: number;      // Expiry timestamp (7 days from iat)
}
```

### Metal Object

```ts
{
  name: string;    // e.g. "Yellow Gold"
  hex: string;     // Color hex e.g. "#FFD700"
  rating: number;  // 1–5 recommendation strength
  note: string;    // Human-readable explanation
}
```

### Clothing Color Object

```ts
{
  name: string;      // e.g. "Terracotta"
  hex: string;       // e.g. "#C65D3C"
  category: string;  // e.g. "Earth", "Bold", "Neutral"
}
```

### Lipstick Object

```ts
{
  name: string;    // e.g. "Warm Coral"
  hex: string;     // e.g. "#FF6B5B"
  finish: string;  // e.g. "Matte", "Gloss", "Satin", "Velvet"
  vibe: string;    // e.g. "Playful", "Sultry", "Natural"
}
```

---

## ⚠️ Error Handling Strategy

The server uses a consistent error response shape across all endpoints:

```json
{ "error": "Human-readable error message" }
```

**HTTP status codes used:**

| Code | Meaning | When used |
|---|---|---|
| `200` | OK | Successful request |
| `400` | Bad Request | Missing or invalid input from client |
| `401` | Unauthorized | Missing, expired, or invalid JWT |
| `404` | Not Found | User or profile doesn't exist |
| `409` | Conflict | Email already registered |
| `500` | Internal Server Error | Unexpected server-side exception |

All route handlers wrap their logic in `try/catch` blocks. If an unexpected error is thrown (e.g. a bug in the analysis engine), the `catch` block returns a `500` with `err.message` so the client receives useful diagnostic information during development.

> **Production hardening tip:** In production, you should avoid returning raw `err.message` values, as these can leak internal implementation details. Replace with a generic message like `"Something went wrong"` and log the full error server-side.

---

## 🔒 Security Considerations

### Current implementation

| Area | Status | Notes |
|---|---|---|
| Password storage | ✅ Secure | bcrypt with cost factor 10 |
| JWT signing | ✅ Secure | HS256 algorithm, 7-day expiry |
| User enumeration | ✅ Protected | Identical error for wrong email/password |
| ID generation | ✅ Secure | UUID v4 (cryptographically random) |
| CORS | ⚠️ Open | Currently allows all origins — restrict in production |
| HTTPS | ⚠️ Not enforced | Add TLS termination (via nginx/Caddy) in production |
| Rate limiting | ❌ Missing | Add `express-rate-limit` to prevent brute-force attacks |
| Input sanitization | ⚠️ Partial | Basic presence checks — add length/format validation |
| Helmet.js | ❌ Missing | Add `helmet` middleware for security headers |
| JWT Secret | ⚠️ Hardcoded | Move to environment variable for production |

### Recommended production additions

```sh
npm install helmet express-rate-limit dotenv
```

```js
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
require('dotenv').config();

// Security headers
app.use(helmet());

// Rate limiting for auth endpoints
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15 minutes
  max: 20,                    // max 20 auth attempts per window
  message: { error: 'Too many attempts. Please try again later.' }
});
app.use('/api/auth', authLimiter);

// Restrict CORS to your frontend domain
app.use(cors({ origin: 'https://your-frontend.com' }));
```

---

## ⚡ Performance Notes

- **In-memory store:** All reads and writes are O(1) for user lookups (keyed by email) and O(n) for profile operations (linear search by UUID). At typical usage scale (hundreds of users, tens of profiles each), this is extremely fast.
- **bcrypt cost factor:** `bcrypt.hash()` is intentionally slow — ~100ms at cost factor 10. This is a security feature, not a bug. It applies only to auth endpoints, not analysis endpoints.
- **JSON body limit:** Set to `10mb` to accommodate base64-encoded avatar images from the canvas. Reduce this limit if avatars are not needed.
- **No database connection overhead:** Because the store is in-memory, there is zero network round-trip for data access. Analysis requests typically complete in under 5ms.
- **Single-threaded:** Node.js is single-threaded. The bcrypt operations use `async/await` to avoid blocking the event loop while hashing.

---

## 🔮 Roadmap & Future Plans

We have big dreams for BeautyKit's backend. Here's what's coming:

### 🗄 Database
- [ ] **MongoDB integration** via Mongoose — persistent user and profile storage
- [ ] **PostgreSQL + Prisma** option — for contributors who prefer relational databases
- [ ] **Redis caching** — cache frequently requested recommendation data

### 🔐 Auth
- [ ] **Refresh token rotation** — more secure token lifecycle management
- [ ] **OAuth 2.0** — Sign in with Google / Apple
- [ ] **Email verification** — confirm email on signup
- [ ] **Password reset flow** — email-based reset with time-limited tokens

### 🎨 Analysis
- [ ] **Improved undertone algorithm** — incorporate perceptual luminance (Rec. 709) instead of arithmetic mean
- [ ] **Seasonal color theory** — classify users into Spring / Summer / Autumn / Winter types
- [ ] **Multiple pixel sampling** — accept an array of RGB samples and compute a weighted average for more accuracy
- [ ] **Foundation shade matching** — map skin tone to specific foundation shades from major brands
- [ ] **AI model integration** — plug in a TensorFlow.js or Python model for image-based skin tone detection

### 💄 Recommendations
- [ ] **Expanded product database** — link recommendations to real products with affiliate links
- [ ] **User feedback loop** — allow users to rate recommendations and improve the engine
- [ ] **Seasonal looks** — different recommendation sets for different seasons or occasions

### 🛠 Infrastructure
- [ ] **TypeScript migration** — full type safety across the codebase
- [ ] **Jest unit tests** — test coverage for `analyzeSkinTone` and `getRecommendations`
- [ ] **Docker container** — one-command deployment with `docker compose up`
- [ ] **OpenAPI / Swagger docs** — auto-generated interactive API documentation
- [ ] **Winston logging** — structured logging for production observability
- [ ] **CI/CD pipeline** — GitHub Actions for lint, test, and deploy

---

## 🤝 Contributing

**BeautyKit is open-source and we warmly welcome all contributions!** Whether you're fixing a typo, improving the skin tone algorithm, adding a new recommendation category, or helping with the database migration — every contribution matters.

### ⭐ First — Star the repo!

If BeautyKit has been useful or interesting to you, **please give it a star on GitHub**. It takes 2 seconds and it means the world to open-source maintainers. It also helps others discover the project.

👉 **[⭐ Star BeautyKit on GitHub](https://github.com/beautykit/BeautyKit)**

### 🍴 Fork it!

Ready to contribute code? Fork the repository and make it your own playground.

👉 **[🍴 Fork BeautyKit on GitHub](https://github.com/beautykit/BeautyKit/fork)**

---

### How to contribute

#### 1. Fork the repository

Click the **Fork** button at the top-right of the GitHub page, or use the GitHub CLI:

```sh
gh repo fork your-username/BeautyKit --clone
```

#### 2. Create a feature branch

```sh
git checkout -b feature/your-feature-name
# or for bug fixes:
git checkout -b fix/description-of-fix
```

**Branch naming conventions:**
| Prefix | Use case |
|---|---|
| `feature/` | New features |
| `fix/` | Bug fixes |
| `docs/` | Documentation changes |
| `refactor/` | Code refactoring (no behavior change) |
| `test/` | Adding or fixing tests |
| `perf/` | Performance improvements |

#### 3. Make your changes

- Keep changes focused — one feature or fix per PR
- Follow the existing code style (2-space indent, single quotes, async/await)
- Add comments to explain non-obvious logic (especially in the analysis engine)
- Update this README if your change affects the API or architecture

#### 4. Test your changes

```sh
# Start the server
npm run dev

# Test the health endpoint
curl http://localhost:5000/api/health

# Test signup
curl -X POST http://localhost:5000/api/auth/signup \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@test.com","password":"test1234"}'

# Test skin analysis
curl -X POST http://localhost:5000/api/analyze-pixels \
  -H "Content-Type: application/json" \
  -d '{"r":198,"g":158,"b":120}'
```

#### 5. Commit with a clear message

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```sh
git commit -m "feat: add rate limiting to auth endpoints"
git commit -m "fix: correct cool undertone boundary condition"
git commit -m "docs: add MongoDB migration guide to README"
git commit -m "perf: use perceptual luminance for depth calculation"
```

#### 6. Push and open a Pull Request

```sh
git push origin feature/your-feature-name
```

Then open a Pull Request on GitHub. In your PR description, please include:

- **What** the change does
- **Why** it was needed
- **How** you tested it
- **Screenshots** (if applicable)

---

### 💡 Good first issues

New to the project? Here are some great starting points:

- 🧪 **Add unit tests** for `analyzeSkinTone()` — write Jest tests that verify undertone and depth classification for known RGB values
- 📝 **Add input validation** — validate that `r`, `g`, `b` are integers between 0–255
- 🔒 **Add Helmet.js** — add security headers middleware and document the change
- 🌍 **Add CORS configuration** — accept an allowed-origins list from environment variables
- 💅 **Add more lipstick shades** — expand the recommendation data for any undertone category
- 🌙 **Add a "seasonal" classification** — map warm/cool/neutral + depth to Spring/Summer/Autumn/Winter

### 🐛 Reporting bugs

Found a bug? Please [open an issue](https://github.com/beautykit/BeautyKit/issues/new) with:

- A clear, descriptive title
- Steps to reproduce
- Expected vs actual behavior
- Node.js version (`node --version`)
- Operating system

### 💬 Suggesting features

Have an idea? [Open a discussion](https://github.com/beautykit/BeautyKit/discussions) or [create a feature request issue](https://github.com/beautykit/BeautyKit/issues/new) with the `enhancement` label.

---

## 📜 Code of Conduct

BeautyKit is committed to fostering a welcoming, inclusive community. We celebrate diversity in all its forms — including the skin tone diversity that inspired this very project.

- Be kind and respectful in all interactions
- Welcome newcomers and beginners with patience
- Give constructive, specific feedback on PRs
- Assume good intent from others
- Zero tolerance for harassment, discrimination, or exclusionary behavior

---

## 📄 License

BeautyKit is released under the **MIT License**.

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

See the full [LICENSE](./LICENSE) file for details.

---

<div align="center">

### 🌸 Built with love for every skin tone on earth 🌍

**If BeautyKit helped you, inspired you, or made you smile — please leave a ⭐ star!**

[![Star on GitHub](https://img.shields.io/github/stars/beautykit/BeautyKit?style=social)](https://github.com/beautykit/BeautyKit/stargazers)
[![Fork on GitHub](https://img.shields.io/github/forks/beautykit/BeautyKit?style=social)](https://github.com/beautykit/BeautyKit/fork)

---

Made with 💄 by the BeautyKit contributors

</div>