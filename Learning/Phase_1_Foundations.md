# 🟡 PHASE 1 — Next.js Foundations (Week 1–3)

| Day | Topic | What to Learn | Apply to TradeMind |
|-----|-------|--------------|-------------------|
| 1 | What is Next.js | SSR vs CSR vs SSG, file-based routing | Why migrate from Vite? |
| 2 | Project Setup | `create-next-app`, folder structure | Init `apps/web` |
| 3 | App Router | `page.tsx`, `layout.tsx`, `loading.tsx` | Dashboard layout |
| 4 | Routing | Static, dynamic routes, route groups | `/dashboard`, `/orders`, `/funds` |
| 5 | Navigation | `<Link>`, `useRouter()`, `redirect()` | Sidebar navigation |
| 6 | Server Components | RSC concept, when to use | WatchList as server component |
| 7 | Client Components | `"use client"`, interactivity | OrderPanel, chat UI |
| 8 | Data Fetching | `fetch` in server components, caching | Holdings from API |
| 9 | Loading & Error | `loading.tsx`, `error.tsx`, Suspense | Add loading skeletons |
| 10 | Metadata & SEO | `metadata` export, `<head>` | SEO for dashboard pages |
| 11 | `next/image` | Optimization, lazy loading | Stock logos, avatars |
| 12 | `next/font` | Google Fonts, zero layout shift | Add Inter/Outfit font |

---

## 📘 Next.js Common Patterns — Notes

### 1. Server-Side Rendering (SSR)
**Definition:** SSR is a technique in which HTML is generated on the server when a page is requested.

- The server generates HTML and sends it to the browser.
- Useful for pages that need **fresh data**.
- Can help SEO (Search Engine Optimization — website ko search engines mein discover karna).

> **Example:** Displaying an updated public profile.

---

### 2. Static Site Generation (SSG)
**Definition:** SSG is a technique in which HTML is generated **before** the user requests the page, usually during the build process.

- The generated HTML can be served quickly.
- Suitable for pages whose content does **not change frequently**.
- Can improve performance and SEO.

> **Example:** About page, help page, or documentation.

---

### 3. SSR vs SSG

| | SSR | SSG |
|---|---|---|
| **When** | HTML generated during a request | HTML generated ahead of time |
| **Best for** | Fresh or request-specific data | Relatively stable content |
| **Server load** | Requires server processing per request | Can serve pre-generated HTML efficiently |

---

### 4. File-System-Based Routing
**Definition:** Next.js creates routes based on the folder and file structure of your project.

In the **App Router**, folders define URL segments and `page.js` defines the page.

```
app/
├── page.js
├── about/
│   └── page.js
└── profile/
    └── page.js
```

- `app/page.js` → `/`
- `app/about/page.js` → `/about`
- `app/profile/page.js` → `/profile`

> **Advantage:** You do not need to manually configure every page route.

---

### 5. Dynamic Routing
**Definition:** Dynamic routing allows one page to handle multiple URLs using a **variable segment**.

```
app/
└── profile/
    └── [id]/
        └── page.js
```

This route can handle:
- `/profile/101`
- `/profile/102`
- `/profile/abhishek`

The value inside `[id]` changes according to the URL.

> **Use case:** User profiles, posts, products, and job details.

---

### 6. API Routes / Route Handlers
**Definition:** API routes allow us to create HTTP endpoints that receive requests and return responses.

In the App Router, these are called **Route Handlers**.

```
app/
└── api/
    └── hello/
        └── route.js
```

```js
export async function GET() {
  return Response.json({
    message: "Hello World",
  });
}
```

Visiting `/api/hello` with a GET request returns JSON.

**Common HTTP methods:**

| Method | Purpose |
|--------|---------|
| `GET` | Retrieve data |
| `POST` | Create data |
| `PUT` | Replace or update data |
| `PATCH` | Partially update data |
| `DELETE` | Delete data |

---

### 7. SEO (Search Engine Optimization)
**Definition:** SEO means improving a website so search engines can discover and understand its content.

- Server-rendered and statically generated HTML can make page content available earlier.
- Next.js provides **metadata** options for page titles and descriptions.
- Rendering strategy alone does **not** guarantee good search rankings.

```js
export const metadata = {
  title: "ProConnect",
  description: "Connect with professionals",
};
```

---

### 8. JavaScript and HTML Rendering

| Technology | Role |
|-----------|------|
| **HTML** | Defines the structure of a webpage |
| **CSS** | Controls the appearance of a webpage |
| **JavaScript** | Adds behavior and interactivity |
| **React** | Builds user interfaces using components |
| **Next.js** | Provides routing, rendering strategies, and other application features on top of React |

Next.js can generate HTML on the server or ahead of time. Client-side JavaScript makes interactive features work.

---

### ⚡ Quick Revision

| Topic | Remember This |
|-------|--------------|
| **SSR** | HTML generated during a request |
| **SSG** | HTML generated ahead of time |
| **Routing** | Folders and files define URLs |
| **Dynamic Routing** | `[id]` represents a variable URL segment |
| **Route Handler** | Creates an API endpoint |
| **SEO** | Helps search engines discover and understand content |
| **React** | Builds component-based interfaces |
| **Next.js** | Adds routing, rendering, and framework features to React |

> 🎯 **Focus most on:** SSR vs SSG, file-system routing, dynamic routes, and Route Handlers — these will be especially useful when building ProConnect.
