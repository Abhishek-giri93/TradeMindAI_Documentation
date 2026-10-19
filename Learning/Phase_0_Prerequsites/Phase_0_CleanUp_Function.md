# 🧹 Cleanup Functions in React (`useEffect`)

---

## 1. What is a Cleanup Function?

A **cleanup function** is a function that you **return from `useEffect`** to undo or stop something that the effect started.

**Examples of what a cleanup function handles:**

| Task Started in Effect | What Cleanup Does |
|---|---|
| Start a timer | Clear the timer |
| Add an event listener | Remove the event listener |
| Subscribe to a data stream | Unsubscribe from it |
| Fetch / async request | Cancel or ignore outdated request |

> **Simple Rule:**
>
> - **Effect** → *Start something*
> - **Cleanup** → *Stop or undo that setup*

---

## 2. Real-Life Analogy 🌀

Think of a **ceiling fan**:

| Action | React Equivalent |
|---|---|
| **Fan starts** (Setup) | Starting a timer inside `useEffect` |
| **Fan stops** (Cleanup) | Clearing the timer when it's no longer needed |

Just like you'd turn off a fan when you leave a room, React runs the cleanup when the component **unmounts** or **re-renders**.

---

## 3. Basic Syntax

```jsx
useEffect(() => {
  // ✅ Setup: start something
  const timer = setInterval(() => {
    console.log("Tick");
  }, 1000);

  // ✅ Cleanup: stop it when done
  return () => {
    clearInterval(timer);
  };
}, [dependency]);
```

---

## 4. When Does the Cleanup Run?

| Scenario | Cleanup Triggered? |
|---|---|
| Component **unmounts** | ✅ Yes |
| Dependency in `[]` **changes** | ✅ Yes (before next effect runs) |
| No dependency array provided | ✅ Yes (on every re-render) |
| Empty `[]` dependency array | ✅ Yes (only on unmount) |

---

> 📌 **Next Step:** See how cleanup works in practice → [Phase_0_Mini-Project.md](./Phase_0_Mini-Project.md)