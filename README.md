# 🚀 TradeMind AI — Learning Roadmap & Project Documentation

> **Goal:** Learn Next.js from scratch → build TradeMind AI v2.0 (AI-powered stock trading platform)  
> **Approach:** Learn one concept per day → apply it directly to the project  
> **Stack:** Next.js 15 · Node.js · Express.js · MySQL · OpenAI GPT-4 · Python ML

---

## 🌐 Live App
- **Frontend:** [trade-mind-ai.vercel.app](https://trade-mind-ai.vercel.app)
- **Backend API:** [trademind-api-2bvf.onrender.com](https://trademind-api-2bvf.onrender.com)
- **Documentation Repo:** [github.com/Abhishek-giri93/TradeMindAI_Documentation](https://github.com/Abhishek-giri93/TradeMindAI_Documentation)

---

## 🗺️ Full Roadmap Overview (Click for Daily Topics)

- [**PHASE 1 │ Next.js Foundations** (Week 1–3)](./Learning/Phase_1_Foundations.md)
- [**PHASE 2 │ Intermediate Next.js** (Week 4–7)](./Learning/Phase_2_Intermediate.md)
- [**PHASE 3 │ Advanced Next.js** (Week 8–12)](./Learning/Phase_3_Advanced.md)
- [**PHASE 4 │ Full-Stack Mastery** (Week 13–16)](./Learning/Phase_4_Full_Stack.md)
- [**PROJECT │ TradeMind AI v2.0** (Week 17–28)](./Learning/Project_TradeMind_v2.md)
- [**PHASE 5 │ Web Core Deep Dive** (Week 29–30)](./Learning/Phase_5_Web_Core_Deep_Dive.md) *(Prerequisites Review)*

---

## 🏛️ Final Architecture

```
┌─────────────────────────────────────────────────────────┐
│              Next.js 15 (Vercel)                        │
│   App Router · RSC · Server Actions · NextAuth.js       │
└───────────────┬─────────────────────────────────────────┘
                │ REST API + WebSocket
┌───────────────▼─────────────────────────────────────────┐
│         Node.js + Express.js (Render)                   │
│   Auth · Orders · Funds · Market Data · AI Routes       │
└──────┬──────────────────────────┬───────────────────────┘
       │                          │
┌──────▼──────┐        ┌──────────▼──────────────────────┐
│  MySQL      │        │   Python FastAPI (Railway)       │
│  AWS RDS    │        │   FinBERT · Prophet · Optimizer  │
└─────────────┘        └──────────┬──────────────────────┘
                                  │
                       ┌──────────▼──────────────────────┐
                       │  External APIs                   │
                       │  Yahoo Finance · NewsAPI         │
                       │  OpenAI GPT-4                   │
                       └─────────────────────────────────┘
```

---

## 🛠️ Full Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | Next.js 15, TypeScript, Tailwind CSS, shadcn/ui |
| **Charts** | TradingView Lightweight Charts |
| **Real-time** | Socket.io |
| **Auth** | NextAuth.js v5 (JWT) |
| **Backend** | Node.js, Express.js, TypeScript |
| **Database** | MySQL on AWS RDS, mysql2 |
| **AI** | OpenAI GPT-4o, LangChain |
| **ML** | Python, FastAPI, Prophet, FinBERT, scipy |
| **Market Data** | Yahoo Finance API (free), NewsAPI |
| **Deployment** | Vercel (web) · Render (api) · Railway (ml) |
| **Monorepo** | Turborepo |

---

## 📖 Daily Learning Resources

| Topic | Best Resource |
|-------|--------------|
| Next.js | [nextjs.org/docs](https://nextjs.org/docs) |
| React | [react.dev](https://react.dev) |
| TypeScript | [typescriptlang.org/docs](https://typescriptlang.org/docs) |
| Tailwind | [tailwindcss.com/docs](https://tailwindcss.com/docs) |
| MySQL | [mysqltutorial.org](https://mysqltutorial.org) |
| Express.js | [expressjs.com](https://expressjs.com) |
| OpenAI | [platform.openai.com/docs](https://platform.openai.com/docs) |
| FastAPI | [fastapi.tiangolo.com](https://fastapi.tiangolo.com) |
| Socket.io | [socket.io/docs](https://socket.io/docs) |
| shadcn/ui | [ui.shadcn.com](https://ui.shadcn.com) |

---

*Last updated: October 2026*
