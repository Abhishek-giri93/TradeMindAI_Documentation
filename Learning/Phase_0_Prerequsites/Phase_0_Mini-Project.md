# ⏱️ Mini Project: React Live Timer

> **Goal:** Build a live timer using `useEffect` and a **cleanup function** to properly manage `setInterval`.

---

## 📋 Features to Implement

| # | Feature | Description |
|---|---|---|
| 1 | ▶️ Start | Begin counting up every second |
| 2 | ⏸️ Pause | Freeze the timer at the current value |
| 3 | 🔄 Reset | Set the timer back to `0` |
| 4 | 🧹 Cleanup | Clear the interval when paused or component unmounts |

---

## 🗂️ Component State

```jsx
const [seconds, setSeconds] = useState(0);        // current timer value
const [isRunning, setIsRunning] = useState(false); // is timer running?
```

---

## ⚙️ useEffect Logic

```jsx
useEffect(() => {
  if (!isRunning) return; // don't start interval if paused

  // ✅ Setup: start the interval
  const interval = setInterval(() => {
    setSeconds((prev) => prev + 1);
  }, 1000);

  // ✅ Cleanup: stop the interval when isRunning changes or component unmounts
  return () => {
    clearInterval(interval);
  };
}, [isRunning]); // re-runs whenever isRunning changes
```

---

## 🎮 Control Handlers

```jsx
const handleStart = () => setIsRunning(true);
const handlePause = () => setIsRunning(false);
const handleReset = () => {
  setIsRunning(false);
  setSeconds(0);
};
```

---

## 🖥️ Full Component

```jsx
import { useState, useEffect } from "react";

function LiveTimer() {
  const [seconds, setSeconds] = useState(0);
  const [isRunning, setIsRunning] = useState(false);

  useEffect(() => {
    if (!isRunning) return;

    const interval = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    return () => clearInterval(interval); // 🧹 Cleanup
  }, [isRunning]);

  return (
    <div>
      <h1>⏱️ {seconds}s</h1>
      <button onClick={() => setIsRunning(true)}>▶️ Start</button>
      <button onClick={() => setIsRunning(false)}>⏸️ Pause</button>
      <button onClick={() => { setIsRunning(false); setSeconds(0); }}>
        🔄 Reset
      </button>
    </div>
  );
}

export default LiveTimer;
```

---

## 🔍 How the Cleanup Works Here

| Trigger | What Happens |
|---|---|
| User clicks **Pause** | `isRunning` → `false`, effect re-runs, cleanup clears the interval |
| User clicks **Reset** | `isRunning` → `false`, same as above + seconds reset to `0` |
| Component **unmounts** | React automatically calls the cleanup → interval cleared |

---

## 🧠 Key Takeaway

> Without the cleanup `return () => clearInterval(interval)`, the interval would keep running **even after pausing** — causing the timer to jump ahead unexpectedly on resume. Cleanup prevents this memory leak and logic bug.

---

> 📌 **Back to theory:** [Phase_0_CleanUp_Function.md](./Phase_0_CleanUp_Function.md)