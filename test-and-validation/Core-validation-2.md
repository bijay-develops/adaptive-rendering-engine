## How could we test this? 

### 2️⃣ Performance Metrics
- TTFB
- FCP
- Memory usage
- CPU usage

### 3️⃣ Cache Behavior
- ISR revalidation
- Cache invalidation
- Cold vs warm cache

### 4️⃣ Load & Stress
- Multiple concurrent requests
- Strategy fallback under load


<hr>

Below is a **complete, zero-cost, Linux-friendly testing methodology** covering **metrics, cache behavior, and load/stress**, written so We can **both implement it and defend it in viva**.

---

# 2️⃣ **Performance Metrics Testing**

We measure **TTFB, FCP, CPU, Memory** using **local tools + instrumentation**.

---

## 2.1 **TTFB (Time To First Byte)**

### What it measures

Server responsiveness + rendering delay.

### Tool (Zero cost)

`curl`

### Command

```bash
curl -w "TTFB: %{time_starttransfer}\n" -o /dev/null -s \
     http://localhost:3000/page
```

### Expected Result

```
TTFB: 0.124
```

### Strategy Comparison

Run the same command with different headers:

```bash
curl -H "X-Network-Speed: slow" \
     -w "TTFB: %{time_starttransfer}\n" \
     -o /dev/null -s http://localhost:3000/page
```

✔ Store results per strategy
✔ Graph later

---

## 2.2 **FCP (First Contentful Paint)**

### Why browser tools are needed

FCP is measured **in the browser**, not server.

### Tool

**Google Chrome** DevTools (free)

### Steps

1. Open DevTools → Performance
2. Enable network throttling
3. Load the page
4. Record **FCP**

### Automation (Optional)

**Lighthouse**

```bash
lighthouse http://localhost:3000/page --only-categories=performance
```

✔ Generates JSON + HTML report
✔ Examiner-friendly evidence

---

## 2.3 **CPU Usage**

### Tool

Linux native: `top` / `htop`

### Command

```bash
htop
```

Watch:

* Node.js process
* CPU spikes during SSR vs SSG vs Streaming

### Advanced (per request)

Log CPU time inside our engine:

```ts
const start = process.cpuUsage();
// render
const end = process.cpuUsage(start);
```

---

## 2.4 **Memory Usage**

### Tool

Linux: `ps`, `htop`

```bash
ps -o pid,rss,cmd -C node
```

Or inside code:

```ts
process.memoryUsage().rss
```

✔ Compare memory usage across strategies

---

# 3️⃣ **Cache Behavior Testing**

---

## 3.1 **Cold Cache vs Warm Cache**

### Cold Cache

```bash
rm -rf cache/*
curl http://localhost:3000/page
```

### Warm Cache

```bash
curl http://localhost:3000/page
```

### Expected Observation

| State | TTFB |
| ----- | ---- |
| Cold  | High |
| Warm  | Low  |

Log output:

```
Cache status: HIT
```

---

## 3.2 **ISR Revalidation Testing**

### Setup

ISR TTL = 10 seconds

### Steps

```bash
curl http://localhost:3000/page
sleep 12
curl http://localhost:3000/page
```

### Expected Behavior

* First request: cached
* Second request: regenerated

Log:

```
ISR revalidation triggered
```

---

## 3.3 **Cache Invalidation**

### Trigger invalidation manually

```bash
curl -X POST http://localhost:3000/invalidate/page
```

### Expected Output

```
Cache invalidated for /page
```

Followed by:

```
Fresh render executed
```

---

# 4️⃣ **Load & Stress Testing**

---

## 4.1 **Multiple Concurrent Requests**

### Tool

Apache Benchmark (`ab`)

```bash
ab -n 1000 -c 50 http://localhost:3000/page
```

### Metrics Observed

* Requests/sec
* Failed requests
* TTFB increase

---

## 4.2 **Strategy Fallback Under Load**

### Our Engine Rule

```text
IF load > threshold → fallback to SSG/ISR
```

### Expected Logs

```
High load detected
Strategy switched: SSR → SSG
```

### Proof

Response header:

```
X-Rendering-Strategy: SSG
```

---

## 4.3 **Stress Testing with Mixed Conditions**

### Bash Script

```bash
for i in {1..100}; do
  curl -H "X-Network-Speed: slow" http://localhost:3000/page &
done
wait
```

Observe:

* CPU spikes
* Strategy switch logs
* Cache utilization

---

# 📊 How We Present Results (Important)

### Tables

| Strategy | TTFB | FCP | CPU | Memory |
| -------- | ---- | --- | --- | ------ |

### Graphs

* Load vs TTFB
* Cache hit ratio
* Strategy selection frequency

---

# 📝 How We Write This in our Report

> “Performance metrics were evaluated using a combination of HTTP timing analysis, browser-based performance auditing, and system-level resource monitoring. Cache behavior was analyzed under cold, warm, and invalidation scenarios. Load testing was performed to observe system stability and adaptive strategy fallback under concurrent request conditions.”

This is **perfect examiner language**.

---

# ✅ Final Summary

✔ Fully offline <br>
✔ Linux native tools <br>
✔ Zero cost <br>
✔ Reproducible <br>
✔ Strong technical justification

---
