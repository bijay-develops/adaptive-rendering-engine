## How could we request multiple requests to chjeck: 
### 1️⃣ Rendering Strategy Switching
- Same page
- Different conditions
- Different rendering strategy

<hr>

Below is a **clean, engineering-grade way** to test **Rendering Strategy Switching** for the **same page** under **different conditions**, using **only free tools on Linux**.

---

# 🎯 Goal Recap

We want to verify that:

> **The same URL/page is rendered differently**
> when request conditions change
> and the **Adaptive Rendering Engine (ARE)** chooses a different strategy.

---

# ✅ Core Idea (Simple but Powerful)

**Same page** <br>
⬇ <br>
**Different request context** <br>
⬇ <br>
**Different rendering strategy selected automatically**

We do **NOT** create multiple pages.
We manipulate **request conditions**.

---

# 🧠 What Conditions Can We Change?

| Condition      | How we simulate         |
| -------------- | ----------------------- |
| Network speed  | Artificial delay        |
| Device type    | Headers                 |
| Cache state    | Cold vs warm cache      |
| Load           | Concurrent requests     |
| Data freshness | Time-based invalidation |

---

# 🧩 1️⃣ Strategy Switching via HTTP Headers (Most Important)

Our **Context Analyzer** reads request headers.

### Example headers We define

```http
X-Network-Speed: slow | medium | fast
X-Device-Type: mobile | desktop
X-Cache-State: fresh | stale
X-Load-Level: low | high
```

These are **controlled inputs** for testing.

---

## 🔧 How to Send Multiple Requests (Linux)

### ✅ Tool 1: `curl` (BEST for clarity)

#### Same page, different conditions

```bash
# Slow network → expect SSG / SSR
curl -H "X-Network-Speed: slow" \
     -H "X-Device-Type: mobile" \
     http://localhost:3000/page

# Fast network → expect CSR
curl -H "X-Network-Speed: fast" \
     -H "X-Device-Type: desktop" \
     http://localhost:3000/page

# Stale cache → expect ISR
curl -H "X-Cache-State: stale" \
     http://localhost:3000/page

# Heavy data → expect Streaming SSR
curl -H "X-Data-Size: heavy" \
     http://localhost:3000/page
```

✔ Same URL <br>
✔ Different headers <br>
✔ Different rendering strategy <br>

---

## 🔍 How We VERIFY Strategy Switching

Our server **logs the chosen strategy**:

```text
[ARE] Request: /page
[ARE] Context: slow-network, mobile
[ARE] Strategy selected: SSG
```

This log is **proof** for examiners.

---

# 🧪 2️⃣ Automated Multiple Requests (Batch Testing)

### Bash script (zero cost)

```bash
#!/bin/bash

strategies=(
  "slow mobile"
  "fast desktop"
  "medium desktop"
)

for s in "${strategies[@]}"; do
  set -- $s
  curl -H "X-Network-Speed: $1" \
       -H "X-Device-Type: $2" \
       http://localhost:3000/page
  echo ""
done
```

✔ Reproducible <br>
✔ Documentable <br>
✔ Examiner-friendly <br>

---

# ⚙️ 3️⃣ Load-Based Strategy Switching

Now test **same page under load**.

### Using Apache Benchmark (`ab`)

```bash
ab -n 1000 -c 50 http://localhost:3000/page
```

Our engine logic:

* Low load → SSR
* High load → SSG / ISR

### Strategy log example

```text
Load detected: HIGH
Strategy switched from SSR → SSG
```

---

# 🧠 4️⃣ Edge vs Origin Simulation (Optional but Powerful)

Run:

* `edge-node` container (low latency)
* `origin` container (higher latency)

Then route traffic.

### Expected behavior

| Node   | Strategy        |
| ------ | --------------- |
| Edge   | SSG / Edge ISR  |
| Origin | SSR / Streaming |

---

# 📊 5️⃣ Visual Proof (Highly Recommended)

Return the **strategy in response headers**:

```http
X-Rendering-Strategy: Streaming-SSR
```

Then:

```bash
curl -I http://localhost:3000/page
```

Output:

```http
X-Rendering-Strategy: ISR
```

This is **bulletproof evidence**.

---

# 🧪 6️⃣ Advanced Testing (Optional)

If We want **graphs & reports**, use **Apache JMeter**:

* Same endpoint
* Different header profiles
* Visual performance charts

But **not required** for final year.

---

# 🧠 How Do We Write This in our Report

> “Rendering strategy switching was validated by issuing multiple HTTP requests to the same endpoint under varying contextual conditions using custom headers. The Adaptive Rendering Engine dynamically selected different rendering strategies, which were logged and returned as response metadata for verification.”

This is **exactly examiner language**.

---

# ✅ Final Summary (Very Clear)

✔ Same page <br>
✔ Different request headers <br>
✔ Different runtime conditions <br>
✔ Different rendering strategy <br>
✔ Fully testable on Linux <br>
✔ Zero cost

---

