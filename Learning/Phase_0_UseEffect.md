# ⚡ React `useEffect` Hook

## What is `useEffect`?

`useEffect` is a React Hook that lets you **synchronize a component with an external system** or perform **side effects** after rendering.

> **Side Effect** — Any work that happens *outside* the normal rendering process, such as:
> - Fetching data from an API
> - Setting up event listeners
> - Interacting with the DOM directly
> - Starting timers or subscriptions
> - Interacting with external systems (WebSockets, localStorage, etc.)

---

## Basic Syntax

```jsx
useEffect(() => {
  // your side effect code here

  return () => {
    // optional cleanup function
  };
}, [dependencies]);
```

| Part | Description |
|---|---|
| `() => { ... }` | The effect function that runs after render |
| `return () => { ... }` | Optional cleanup function (runs before next effect or on unmount) |
| `[dependencies]` | Array that controls *when* the effect re-runs |

---

## Types of `useEffect` — Based on the Dependency Array

The behavior of `useEffect` is entirely controlled by what you pass as the **dependency array**.

---

### 1. 🔄 No Dependency Array — Runs on Every Render

```jsx
useEffect(() => {
  console.log("Effect executed");
});
```

**When it runs:**
- ✅ After the initial render
- ✅ After every state update
- ✅ After every prop change that causes a re-render

> ⚠️ **Warning:** If you update state inside this kind of effect without any condition, it will cause an **infinite render loop** because updating state triggers a re-render, which triggers the effect again.

---

### 2. 🚀 Empty Dependency Array `[]` — Runs Only Once (On Mount)

```jsx
useEffect(() => {
  console.log("Runs only once — on mount");
}, []);
```

**When it runs:**
- ✅ Only after the **first render** (component mounts)
- ❌ Does NOT re-run on subsequent renders

**Common use case:** Fetching initial data, setting up subscriptions.

---

### 3. 🎯 With Specific Dependencies — Runs When Dependencies Change

```jsx
useEffect(() => {
  console.log("count changed:", count);
}, [count]);
```

**When it runs:**
- ✅ After the initial render
- ✅ Whenever `count` changes
- ❌ Does NOT run if other state/props change

**Common use case:** Reacting to a specific value change (e.g., re-fetching data when a search query changes).

---

## 🧹 Cleanup Function

The cleanup function runs **before** the effect re-runs or when the component **unmounts**. It's used to avoid memory leaks.

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Tick");
  }, 1000);

  return () => {
    clearInterval(timer); // cleanup: stop the timer
  };
}, []);
```

**Common cleanup scenarios:**
- Clearing timers (`clearInterval`, `clearTimeout`)
- Unsubscribing from subscriptions
- Removing event listeners
- Cancelling fetch requests

---

## 📊 Summary Table

| Dependency Array | Runs After Mount | Runs After Update | Cleanup Runs |
|---|---|---|---|
| Nothing (omitted) | ✅ | ✅ Every render | ✅ Before each re-run |
| `[]` (empty) | ✅ | ❌ | ✅ On unmount only |
| `[value]` (specific) | ✅ | ✅ When `value` changes | ✅ Before next run |

---

## 💡 Common Patterns & Examples

### Fetching Data on Mount

```jsx
useEffect(() => {
  async function fetchData() {
    const res = await fetch("https://api.example.com/data");
    const data = await res.json();
    setData(data);
  }
  fetchData();
}, []); // empty array = fetch only once
```

### Event Listener Setup & Cleanup

```jsx
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);
  window.addEventListener("resize", handleResize);

  return () => {
    window.removeEventListener("resize", handleResize); // cleanup
  };
}, []);
```

### Reacting to a Value Change

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]); // runs every time 'count' changes
```

---

## ⚠️ Common Mistakes

| Mistake | Problem | Fix |
|---|---|---|
| Updating state inside an effect with no deps | Infinite loop | Add proper dependencies or a condition |
| Missing a dependency in the array | Stale data / stale closures | Include all used variables in the dep array |
| Not cleaning up subscriptions/timers | Memory leaks | Always return a cleanup function |
| Using `async` directly in `useEffect` | Syntax error | Define an `async` function inside and call it |

---

## 🔗 Quick Reference

```
No deps array   → runs every render
[]              → runs once on mount
[a, b]          → runs when a or b changes
return () => {} → cleanup before next run or unmount
```
