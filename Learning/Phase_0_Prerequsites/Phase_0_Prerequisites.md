# 🟦 PHASE 0 — Prerequisites (Week 1–2)

> Make these solid before touching Next.js

| Day | Topic | What to Learn | Apply to TradeMind |
|-----|-------|--------------|-------------------|
| 1 | HTML Semantics | Semantic tags, forms, tables | Dashboard HTML skeleton |
| 2 | CSS Fundamentals | Flexbox, Grid, Variables | Navbar + sidebar layout |
| 3 | CSS Advanced | Animations, transitions, responsive | Price flash animations |
| 4 | JS ES6+ | Arrow functions, destructuring, spread | Clean up existing JS |
| 5 | JS Async | Promises, async/await, fetch | API call patterns |
| 6 | JS Modules | import/export, named vs default | Refactor components |
| 7 | React Basics | JSX, components, props | Review existing components |
| 8 | React Hooks | useState, useEffect, useContext | Fix state bugs |
| 9 | React Patterns | Lifting state, composition | Order + WatchList state |
| 10 | Git & GitHub | branches, PRs, commits | Daily pushes to this repo |

---

## 📝 Lesson Notes: Days 8 & 9 (React Hooks & Patterns)

### 🎣 React Hooks (The Engine of React)
Hooks let you "hook into" React state and lifecycle features from inside functional components.

**1. `useState` (Remembering Things)**
Whenever a component needs to remember a value that changes over time, use `useState`.
```jsx
import { useState } from 'react';

export default function StockPrice() {
  const [price, setPrice] = useState(150.00); // state variable and updater function

  return (
    <div>
      <h2>Apple (AAPL): ${price}</h2>
      <button onClick={() => setPrice(152.50)}>Update to Live Price</button>
    </div>
  );
}
```

**2. `useEffect` (Mastering Side Effects & Synchronization)**

`useEffect` allows functional components to synchronize with external systems (APIs, WebSockets, timers, browser DOM). It handles "side effects"—operations that occur outside React's pure rendering cycle.

---

#### 🟢 Phase 1 — Fundamentals
*Topics 1–3 · Understand the basic behavior · Beginner*

- **Topic 1: What is `useEffect` & Side Effects**
  - **Concept**: React components must remain pure functions during rendering (calculating UI from props and state). Side effects—such as fetching API data, establishing WebSocket connections, setting timers, or manually manipulating the DOM—must happen outside render flow.
  - **Execution Timing**: `useEffect` runs **after** the browser paints the UI to the screen. This guarantees that side-effect workloads do not freeze visual rendering.
  - **Mental Flow**: `Render UI` ➡️ `Commit to DOM` ➡️ `Browser Paints Screen` ➡️ `useEffect Executes`.

- **Topic 2: Syntax and Anatomy**
  - **Structure**:
    ```jsx
    useEffect(() => {
      // Setup / Effect logic (runs after screen paint)

      return () => {
        // Optional cleanup logic (runs before re-running effect or unmounting)
      };
    }, [dependencies]); // Optional dependency array
    ```
  - **The 2 Arguments**:
    1. **Setup function**: The function containing your side-effect logic.
    2. **Dependency array** *(optional)*: List of reactive values (state, props) that trigger the effect when modified.

- **Topic 3: Effect without Dependencies (Runs on Every Render)**
  - **Syntax**: `useEffect(() => { ... })` *(dependency array omitted)*.
  - **Behavior**: Executes after the initial mount **and after every single re-render** of the component.
  - **Code Example**:
    ```jsx
    useEffect(() => {
      // Useful for global debugging / logging render events
      console.log(`[TradeMind] Dashboard rendered at ${new Date().toLocaleTimeString()}`);
    });
    ```
  - ⚠️ **Critical Trap (Infinite Render Loop)**: If you update state inside an effect without dependencies (`setCount(c => c + 1)`), setting state triggers a re-render, which re-runs the effect, triggering another re-render infinitely.

---

#### 🟡 Phase 2 — Dependencies and Lifecycle
*Topics 4–6 · Understand when effects execute · Beginner–Intermediate*

- **Topic 4: Empty Dependency Array `[]` (Mounting Behavior)**
  - **Syntax**: `useEffect(() => { ... }, [])`
  - **Behavior**: Executes **exactly once** after the initial render (mount). It never re-runs during subsequent state/prop changes.
  - **Real-World Use**: One-time initializations (e.g., loading user profile, verifying auth session, or fetching market operating hours).
  - **Code Example**:
    ```jsx
    useEffect(() => {
      async function loadMarketStatus() {
        const response = await fetch('/api/market/status');
        const data = await response.json();
        setIsMarketOpen(data.isOpen);
      }
      loadMarketStatus();
    }, []); // Empty array = run ONCE on mount
    ```

- **Topic 5: Specific Dependencies `[dep1, dep2]` (Re-running on Changes)**
  - **Syntax**: `useEffect(() => { ... }, [symbol, timeframe])`
  - **Behavior**: Runs on initial mount, and re-executes **only when at least one dependency value changes** between renders.
  - **Equality Check**: React checks dependencies using `Object.is()` (shallow comparison).
  - **Code Example (Fetching Stock Chart on Ticker Change)**:
    ```jsx
    useEffect(() => {
      async function fetchStockHistory() {
        setLoading(true);
        const res = await fetch(`/api/charts?symbol=${symbol}&timeframe=${timeframe}`);
        const chartData = await res.json();
        setCandles(chartData);
        setLoading(false);
      }
      fetchStockHistory();
    }, [symbol, timeframe]); // Re-executes only when symbol or timeframe changes
    ```

- **Topic 6: Mental Model: Synchronization vs. Lifecycle**
  - **Shift in Mindset**: In legacy class components, developers thought in lifecycles (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`).
  - **Modern React (Synchronization)**: Think: *"How do I synchronize this component with an external system as long as specific props/state are current?"*
  - **Exhaustive Dependencies Rule**: Any reactive value (state, props, or variables computed from them) used inside the effect **must** be listed in the dependency array to prevent stale closure bugs.

---

#### 🟠 Phase 3 — Cleanup and Real-World Tasks
*Topics 7–9 · Manage timers, events and API requests · Intermediate*

- **Topic 7: The Cleanup Function**
  - **Syntax**: Return a function from the effect callback: `return () => { /* cleanup */ };`.
  - **When Cleanup Runs**:
    1. **Before re-executing**: Prior to the effect running with new dependency values, React runs the previous cleanup to tear down old listeners/timers.
    2. **On unmount**: When the component is removed from the DOM.
  - **Why Cleanup is Critical**: Prevents memory leaks, stale callbacks, and memory leaks from updating state on unmounted components.
  - **React 18 Strict Mode Note**: During local development, React intentionally mounts ➡️ unmounts ➡️ remounts components to verify cleanup reliability.

- **Topic 8: Managing Timers & Intervals**
  - **Handling `setInterval` / `setTimeout`**: Always clear timers inside the cleanup function.
  - **Functional State Updates**: When updating state inside a timer, use the callback pattern `setCount(prev => prev + 1)` rather than reading state directly to avoid stale closures.
  - **Code Example (Active Order Expiry Countdown)**:
    ```jsx
    useEffect(() => {
      if (!isOrderActive) return;

      const timerId = setInterval(() => {
        setCountdown((prev) => {
          if (prev <= 1) {
            clearInterval(timerId);
            return 0;
          }
          return prev - 1;
        });
      }, 1000);

      // Cleanup: Cancels interval if order settles or user navigates away
      return () => clearInterval(timerId);
    }, [isOrderActive]);
    ```

- **Topic 9: Event Listeners, WebSockets & API Race Conditions**
  - **Window / DOM Events**: Register listeners in the effect and unregister in cleanup (`window.addEventListener` / `window.removeEventListener`).
  - **WebSockets**: Connect on mount or symbol change, and close the socket connection in cleanup.
  - **Race Condition Prevention with `AbortController`**: When dependencies update quickly (e.g., user rapidly switching stocks from `AAPL` to `TSLA`), in-flight requests may finish out of order. An `AbortController` ensures stale requests are discarded.
  - **Code Example (Cancelable Data Fetch)**:
    ```jsx
    useEffect(() => {
      const controller = new AbortController();

      async function fetchLiveQuote() {
        try {
          const res = await fetch(`/api/quote/${symbol}`, {
            signal: controller.signal,
          });
          const quote = await res.json();
          setPrice(quote.price);
        } catch (err) {
          if (err.name !== 'AbortError') {
            console.error('Fetch failed:', err);
          }
        }
      }

      fetchLiveQuote();

      // Cleanup: Aborts pending request if symbol changes or component unmounts
      return () => controller.abort();
    }, [symbol]);
    ```

---

#### 🔴 Phase 4 — Advanced Concepts
*Topics 10–12 · Debugging, optimization and project implementation · Intermediate–Advanced*

- **Topic 10: Infinite Loops & Common Pitfalls**
  - **Pitfall 1 — Object/Array Dependencies**:
    ```jsx
    // ❌ BAD: New object reference created on every render triggers infinite effect loop
    const filterOptions = { market: 'NASDAQ', minVolume: 10000 };
    useEffect(() => { fetchStocks(filterOptions); }, [filterOptions]);

    // ✅ GOOD: Pass primitive values or memoize with useMemo
    useEffect(() => { fetchStocks({ market, minVolume }); }, [market, minVolume]);
    ```
  - **Pitfall 2 — Function Dependencies**:
    ```jsx
    // ❌ BAD: Function redefined every render causes continuous triggers
    function loadOrderBook() { ... }
    useEffect(() => { loadOrderBook(); }, [loadOrderBook]);

    // ✅ GOOD: Move function inside useEffect or wrap in useCallback
    useEffect(() => {
      function loadOrderBook() { ... }
      loadOrderBook();
    }, [symbol]);
    ```

- **Topic 11: Optimization & When NOT to Use `useEffect`**
  - **Don't use for calculating rendering data**: Compute values directly during render:
    ```jsx
    // ❌ Redundant effect:
    useEffect(() => { setTotal(price * shares); }, [price, shares]);
    // ✅ Compute directly in render calculation:
    const total = price * shares;
    ```
  - **Don't use for handling user interactions**: Handle actions directly in button `onClick` or form `onSubmit` events instead of setting a flag state to trigger an effect.
  - **Extract Custom Hooks**: Package complex effect logic into reusable custom hooks (e.g., `useStockTicker(symbol)`, `useMarketStream()`).

- **Topic 12: Real-World TradeMind AI Implementation**
  - **Production Example: Live Stock Ticker Streamer**:
    ```jsx
    import { useState, useEffect } from 'react';

    export default function LiveTicker({ symbol }) {
      const [quote, setQuote] = useState(null);
      const [status, setStatus] = useState('connecting');

      useEffect(() => {
        let isMounted = true;
        const controller = new AbortController();

        // 1. Initial REST snapshot fetch with cancellation
        async function fetchInitialQuote() {
          try {
            const res = await fetch(`/api/quote/${symbol}`, { signal: controller.signal });
            const data = await res.json();
            if (isMounted) setQuote(data);
          } catch (err) {
            if (err.name !== 'AbortError') console.error('Snapshot error:', err);
          }
        }
        fetchInitialQuote();

        // 2. Real-time WebSocket live price stream
        const socket = new WebSocket(`wss://api.trademind.ai/live-feed?symbol=${symbol}`);

        socket.onopen = () => isMounted && setStatus('connected');
        socket.onmessage = (event) => {
          const liveTick = JSON.parse(event.data);
          if (isMounted) setQuote((prev) => ({ ...prev, ...liveTick }));
        };
        socket.onerror = () => isMounted && setStatus('error');

        // 3. Complete Cleanup (aborts REST call, closes socket, drops state updates)
        return () => {
          isMounted = false;
          controller.abort();
          socket.close();
        };
      }, [symbol]); // Resubscribes seamlessly when symbol changes

      return (
        <div className="ticker-badge">
          <h3>{symbol} Feed ({status})</h3>
          <p className="price">${quote ? quote.price : 'Loading live price...'}</p>
          {quote?.change && <p className="change">{quote.change}%</p>}
        </div>
      );
    }
    ```


**3. `useContext` (Teleporting Data)**
Context allows you to share data globally across your app without passing props manually down multiple levels.
```jsx
import { useContext, createContext } from 'react';

const AuthContext = createContext();

export default function App() {
  return (
    <AuthContext.Provider value={{ user: "Abhishek", balance: 5000 }}>
      <Navbar />
    </AuthContext.Provider>
  );
}

function Navbar() {
  const auth = useContext(AuthContext); // Teleport the data directly!
  return <nav>Welcome, {auth.user}! Funds: ${auth.balance}</nav>;
}
```

### 🧩 React Patterns (Writing Clean Code)

**1. Lifting State Up**
If multiple components need to share state (e.g., a `WatchList` and an `OrderPanel` both needing the selected stock), lift the state to their nearest common parent component.
```jsx
function Dashboard() {
  // LIFTED STATE: The parent owns the state
  const [selectedStock, setSelectedStock] = useState('AAPL');

  return (
    <div className="flex">
      {/* Pass the state DOWN as props */}
      <WatchList selectedStock={selectedStock} onSelect={setSelectedStock} />
      <OrderPanel selectedStock={selectedStock} />
    </div>
  );
}
```

**2. Component Composition (Slots)**
Instead of passing many props to configure a component, pass components *inside* other components using the `children` prop.
```jsx
function Card({ title, children }) {
  return (
    <div className="bg-gray-800 p-4 rounded-lg">
      <h3 className="text-white">{title}</h3>
      <div className="mt-2">{children}</div>
    </div>
  );
}

// Usage:
export default function Portfolio() {
  return (
    <Card title="AI Analysis">
      <p className="text-green-400">GPT-4 predicts a breakout pattern!</p>
    </Card>
  );
}
```
