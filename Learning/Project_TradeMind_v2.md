# 🤖 PROJECT PHASE — TradeMind AI v2.0 (Week 17–28)

## 🏗️ Sprint 1 — Stabilize Existing App (Week 17–18)
| Day | Task | Description |
|-----|------|-------------|
| 51 | Audit Auth | Test register, login, JWT in production |
| 52 | Fix Register | Ensure password validation + DB insert works |
| 53 | Fix Login | JWT generation + response format |
| 54 | Fix Funds | Balance fetch, deposit, withdraw with DB transactions |
| 55 | Fix Orders | Create order, execute, update holdings |
| 56 | Fix Holdings | Read holdings after every trade |
| 57 | Fix CORS & Env | Production environment variables |
| 58 | End-to-end Test | Manual test all features on live app |

## 🔄 Sprint 2 — Next.js Migration (Week 19–20)
| Day | Task | Description |
|-----|------|-------------|
| 59 | Init Next.js App | `create-next-app` in `apps/web` |
| 60 | Migrate Layout | TopBar + Sidebar as `layout.tsx` |
| 61 | Migrate WatchList | Live data hooks added |
| 62 | Migrate OrderPanel | Buy/Sell with form validation |
| 63 | Migrate Funds | Deposit/withdraw + history |
| 64 | Migrate Holdings | P&L calculations from backend |
| 65 | Auth Pages | Login/Register with NextAuth.js |
| 66 | Route Protection | Middleware for dashboard guard |

## 📈 Sprint 3 — Live Market Data (Week 21)
| Day | Task | Description |
|-----|------|-------------|
| 67 | Yahoo Finance API | Real NSE/BSE stock prices |
| 68 | Market Route | `GET /market/quote/:symbol` in Express |
| 69 | TradingView Charts | Candlestick chart component |
| 70 | Socket.io Server | Broadcast live prices every 5s |
| 71 | Socket.io Client | Connect in Next.js, update watchlist |

## 🧠 Sprint 4 — AI Features (Week 22–23)
| Day | Task | Description |
|-----|------|-------------|
| 72 | OpenAI Setup | API key, client config |
| 73 | Stock Analyzer | GPT-4 generates Buy/Sell/Hold signal |
| 74 | AI Signal Card | React component for signal display |
| 75 | AI Chat Backend | Streaming `/ai/chat` route |
| 76 | AI Chat UI | Streaming chat interface in Next.js |
| 77 | News Sentiment | NewsAPI + sentiment scoring |
| 78 | Trading Journal | Write + AI review your trades |

## 🤖 Sprint 5 — Python ML Service (Week 24–25)
| Day | Task | Description |
|-----|------|-------------|
| 79 | FastAPI Setup | Python ML microservice init |
| 80 | FinBERT Sentiment | HuggingFace model for news NLP |
| 81 | Prophet Prediction | 7-day stock price forecast |
| 82 | Portfolio Optimizer | Modern Portfolio Theory (scipy) |
| 83 | Risk Score Engine | Concentration + volatility risk |
| 84 | Deploy ML Service | Push to Railway / Render |

## 🎨 Sprint 6 — UI/UX Polish (Week 26)
| Day | Task | Description |
|-----|------|-------------|
| 85 | Dark Mode System | CSS variables, fintech color palette |
| 86 | Price Animations | Green/red flash on price change |
| 87 | Skeleton Loaders | Loading states for all data |
| 88 | Mobile Responsive | Full mobile layout |
| 89 | Portfolio Chart | Area chart with gradient |
| 90 | Micro-animations | Hover effects, transitions |

## 🚀 Sprint 7 — Production & Polish (Week 27–28)
| Day | Task | Description |
|-----|------|-------------|
| 91 | Unit Tests | Jest for funds + order logic |
| 92 | E2E Tests | Playwright: auth → order flow |
| 93 | Security Audit | Helmet, rate-limit, input sanitize |
| 94 | Performance Audit | Lighthouse > 90 score |
| 95 | Sentry Integration | Production error monitoring |
| 96 | Final Deployment | Vercel + Render + Railway all green |
| 97 | Architecture Docs | Update this repo with diagrams |
| 98 | Demo Video | Record walkthrough of all features |
