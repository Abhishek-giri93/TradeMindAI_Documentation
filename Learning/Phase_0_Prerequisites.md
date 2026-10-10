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

**2. `useEffect` (Doing Side Quests)**
If you need to do something "outside" of rendering—like fetching data from an API or setting up a WebSocket for live prices—use `useEffect`.
```jsx
import { useState, useEffect } from 'react';

export default function Dashboard() {
  const [holdings, setHoldings] = useState([]);

  useEffect(() => {
    // Runs automatically when the component mounts
    async function fetchHoldings() {
      const response = await fetch('/api/holdings');
      const data = await response.json();
      setHoldings(data);
    }
    fetchHoldings();
  }, []); // Empty array = run ONCE on mount

  return <div>You have {holdings.length} stocks in your portfolio.</div>;
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
