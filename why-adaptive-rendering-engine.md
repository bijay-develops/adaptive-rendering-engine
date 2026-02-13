Given that **we are a student**, **no budget**, and We want to **build real technology (not an app)**, the choice must be:

✔ technically deep <br>
✔ feasible solo <br>
✔ impressive to examiners <br>
✔ fully open-source <br>
✔ runnable on a laptop (no paid infra)

---

## 🚀 **Adaptive Rendering Engine (ARE)**

### *“A Zero-Cost, Open-Source Runtime That Automatically Chooses the Best Rendering Strategy”*

---

## 🔑 Why THIS is the best choice for US?

### ✅ Student-feasible

* No need for paid cloud, CDN, or edge providers
* Runs fully on:

  * localhost
  * Docker
  * simulated edge nodes
* We can **mock edge behavior**

### ✅ Zero budget

* Uses only:

  * Node.js / Deno
  * Open-source frameworks
  * Browser APIs
* No SaaS, no Vercel, no AWS

### ✅ Technology (not project)

We are building:

* a **runtime**
* a **decision engine**
* a **performance optimizer**

Not a website.

### ✅ Examiner-friendly

This theme touches:

* Operating Systems (scheduling)
* Distributed Systems
* Compiler theory (decision rules)
* Networking
* Performance engineering

---

## 🧠 What EXACTLY we will build (realistic scope)

### Core Output

A **Rendering Strategy Runtime** that decides, *per request*:

| Condition         | Strategy             |
| ----------------- | -------------------- |
| Static + fresh    | SSG                  |
| Dynamic           | SSR                  |
| Heavy data        | Streaming SSR        |
| Cached but stale  | ISR                  |
| Interactive       | CSR                  |
| Edge-like request | Edge ISR (simulated) |

---

## 🏗 System Architecture (Student-Scale)

```
Request
 ↓
Context Analyzer
   ├─ Network speed
   ├─ Device type
   ├─ Cache state
   ├─ Data volatility
 ↓
Decision Engine (Rules / Heuristics)
 ↓
Renderer Adapter
   ├─ SSG module
   ├─ SSR module
   ├─ Streaming SSR module
   ├─ ISR module
   ├─ CSR module
 ↓
Response + Metrics Logger
```

Everything runs locally.

---

## 🔧 Technology Stack (100% Free)

| Layer      | Tool                        |
| ---------- | --------------------------- |
| Runtime    | Node.js                     |
| Frontend   | React                       |
| SSR/SSG    | Next.js (open-source)       |
| Streaming  | Native Node streams         |
| Cache      | File system / Redis (local) |
| Metrics    | Lighthouse + custom logs    |
| Simulation | Network throttling          |

---

## 📊 What makes this a **TECHNOLOGY project**

We will **not** build pages.
We will build:

✔ Rendering decision algorithm
✔ Pluggable rendering modules
✔ Performance measurement system
✔ Strategy comparison engine

---

## 📄 Thesis-Grade Problem Statement

> *“Current web frameworks require developers to statically choose rendering strategies, leading to sub-optimal performance across diverse network, device, and traffic conditions. This project proposes an Adaptive Rendering Engine that dynamically selects rendering strategies at runtime to optimize performance and resource usage.”*

---

## 📈 Evaluation Metrics (Examiners LOVE this)

* Time To First Byte (TTFB)
* First Contentful Paint (FCP)
* JS bundle size
* CPU usage
* Memory usage
* Strategy decision accuracy

---

## 🧪 What We Will DEMONSTRATE

* Same page rendered using:

  * SSG
  * SSR
  * Streaming SSR
  * ISR
* Engine automatically choosing the best
* Graphs proving improvement

---
