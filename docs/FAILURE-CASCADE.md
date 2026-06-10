# FAILURE CASCADE ANALYSIS

## 1. Traffic Simulation Math

### Scenario

- Notification Recipients: 180,000,000
- Click Through Rate: 8%
- Users Opening App: 14,400,000

For simplicity we assume:

- Active Users: 10,000,000

Each user performs:

1. GET /restaurants
2. GET /restaurant/:id
3. POST /orders

Total API Calls:

10,000,000 × 3 = 30,000,000 requests

Peak RPS:

30,000,000 ÷ 60

= 500,000 Requests Per Second

---

## 2. Component Capacity Numbers

### PostgreSQL

- max_connections = 100

### Node.js

- Capacity ≈ 12,000 RPS

### Payment Calls

- 200ms–2000ms

### Server RAM

- 4GB

### Database Pool Math

Assumptions:

- Query Time = 20ms
- Payment Hold Time = 800ms
- Payment Requests = 30%

Connections Held:

(0.7 × RPS × 0.02)
+
(0.3 × RPS × 0.8)

= 0.254 × RPS

Pool Exhaustion:

100 / 0.254

≈ 394 RPS

Therefore PostgreSQL exhausts around 394 RPS.

---

## 3. Failure Cascade

### Failure 1: PostgreSQL Connection Pool Exhaustion

Severity: CRITICAL

Trigger: ~394 RPS

Impact:

- New requests cannot obtain connections.
- Database rejects requests.

Causes:

- Node request queues increase.

---

### Failure 2: Node.js Event Loop Saturation

Severity: CRITICAL

Trigger: ~12,000 RPS

Impact:

- Response times increase.
- Event loop backs up.

Causes:

- Memory usage grows rapidly.

---

### Failure 3: Synchronous Payment Calls

Severity: HIGH

Trigger: Heavy Order Traffic

Impact:

- Connections remain occupied.
- Database pool exhausts faster.

Causes:

- Payment gateway delays.

---

### Failure 4: Promo Code Race Condition

Severity: HIGH

Impact:

- Oversold discounts.
- Budget exceeded.

Cause:

- Separate SELECT and UPDATE operations.

---

### Failure 5: Static Asset Saturation

Severity: HIGH

Impact:

- Images consume all bandwidth.
- API becomes unreachable.

Cause:

- No CDN.

---

## 4. Incident Timeline

T+0s : Push notification sent

T+3s : PostgreSQL pool exhausted

T+5s : Node.js request queue growing

T+8s : Database rejects connections

T+10s : Payment requests timing out

T+12s : Promo budget oversold

T+15s : Network saturation

T+18s : Node.js OOM crash

T+20s : Service unavailable

T+45m : Root cause identified

T+2h : System restored