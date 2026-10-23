# ?? useContext Hook — Step by Step Notes

> **Category:** React Hooks | **Level:** Intermediate  
> **Prerequisite:** useState, props, component tree understanding

---

## ?? What Problem Does It Solve?

Without `useContext`, passing data from a **parent ? grandchild** component requires drilling props through every middle component. This is called **Prop Drilling** and it gets messy fast.

```
App (has user data)
 +-- Layout
      +-- Sidebar
           +-- UserAvatar  ? needs user data
```

Without context, `user` must pass through `Layout` and `Sidebar` even if they don't use it.  
With `useContext`, `UserAvatar` can grab `user` directly. ?

---

## ?? Step 1 — Create a Context

```jsx
// context/UserContext.js
import { createContext } from "react";

const UserContext = createContext(null); // null is the default value

export default UserContext;
```

> ?? **Tip:** Always create context in a separate file so it's reusable across the app.

---

## ??? Step 2 — Provide the Context (Wrap with Provider)

The **Provider** makes the value available to all children inside it.

```jsx
// App.jsx
import UserContext from "./context/UserContext";

function App() {
    const user = { name: "Abhishek", role: "admin" };

    return (
        <UserContext.Provider value={user}>
            <Layout />
        </UserContext.Provider>
    );
}
```

> ?? **Important:** Only components **inside** the Provider can access the context value.

---

## ?? Step 3 — Consume the Context with useContext

```jsx
// components/UserAvatar.jsx
import { useContext } from "react";
import UserContext from "../context/UserContext";

function UserAvatar() {
    const user = useContext(UserContext); // ? directly grab the value

    return <p>Welcome, {user.name}!</p>;
}
```

No props needed. No drilling. Clean and simple. ?

---

## ?? Step 4 — Dynamic Values (with useState)

Context becomes powerful when combined with `useState` — values update everywhere automatically.

```jsx
// App.jsx
import { useState } from "react";
import UserContext from "./context/UserContext";

function App() {
    const [theme, setTheme] = useState("dark");

    return (
        <UserContext.Provider value={{ theme, setTheme }}>
            <Layout />
        </UserContext.Provider>
    );
}
```

```jsx
// components/ThemeToggle.jsx
import { useContext } from "react";
import UserContext from "../context/UserContext";

function ThemeToggle() {
    const { theme, setTheme } = useContext(UserContext);

    return (
        <button onClick={() => setTheme(theme === "dark" ? "light" : "dark")}>
            Current Theme: {theme}
        </button>
    );
}
```

---

## ?? Step 5 — Clean Pattern: Custom Context Hook

Wrap `useContext` in a custom hook for cleaner, safer usage.

```jsx
// context/UserContext.js
import { createContext, useContext, useState } from "react";

const UserContext = createContext(null);

// ? Custom Provider Component
export function UserProvider({ children }) {
    const [user, setUser] = useState({ name: "Abhishek", role: "admin" });

    return (
        <UserContext.Provider value={{ user, setUser }}>
            {children}
        </UserContext.Provider>
    );
}

// ? Custom Hook — safer to use in components
export function useUser() {
    const context = useContext(UserContext);
    if (!context) {
        throw new Error("useUser must be used inside <UserProvider>");
    }
    return context;
}
```

```jsx
// App.jsx
import { UserProvider } from "./context/UserContext";

function App() {
    return (
        <UserProvider>
            <Layout />
        </UserProvider>
    );
}
```

```jsx
// Any component
import { useUser } from "../context/UserContext";

function Profile() {
    const { user } = useUser(); // ? clean, protected usage
    return <h2>{user.name}</h2>;
}
```

---

## ?? Quick Reference — Full Flow

```
1. createContext()        ? Creates the context object
2. <Context.Provider>     ? Wraps children, supplies the value
3. useContext(Context)    ? Reads the value inside any child
4. useState + Context     ? Makes value reactive (updates everywhere)
5. Custom Hook            ? Cleaner, safer access pattern
```

---

## ? Common Use Cases

| Use Case           | What to Store in Context          |
|--------------------|-----------------------------------|
| Auth / User info   | { user, login, logout }           |
| Theme              | { theme, setTheme }               |
| Language / i18n    | { lang, setLang }                 |
| Shopping Cart      | { cart, addItem, removeItem }     |
| Notifications      | { alerts, addAlert }              |

---

## ?? What NOT to Do

```jsx
// ? BAD — Don't put everything in one global context
// This causes unnecessary re-renders across the whole app
const AppContext = createContext({ user, theme, cart, notifications });

// ? GOOD — Separate contexts for separate concerns
<UserProvider>
    <ThemeProvider>
        <CartProvider>
            <App />
        </CartProvider>
    </ThemeProvider>
</UserProvider>
```

---

## ?? When NOT to Use useContext

- For **frequently updating data** (e.g., mouse position, scroll) ? use local state instead
- For **server data** (API calls) ? use React Query / SWR
- For **very complex state** ? consider Zustand or Redux

---

## ?? Mini Practice Exercise

Build a **Dark/Light Mode Toggle** using `useContext`:

1. Create `ThemeContext` with `createContext()`
2. Wrap `App` in `<ThemeProvider>` with `useState("dark")`
3. In `Navbar`, use `useContext` to show current theme
4. In `ThemeToggle` button, call `setTheme` to switch
5. Apply `className={theme}` on the root `<div>` to change styles

---

## ?? How It Connects to TradeMind

In TradeMindAI, `useContext` will be used for:
- **Auth Context** ? `{ user, token, logout }` shared across the dashboard
- **Theme Context** ? Dark/light mode toggle in sidebar
- **Portfolio Context** ? Holdings data accessible in widgets without prop drilling

---

*?? Date Added: October 23, 2026*  
*??? Tags: React, Hooks, useContext, State Management, Context API*
