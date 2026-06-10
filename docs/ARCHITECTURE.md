# ARCHITECTURE REDESIGN

## Current Architecture

```text
Users
  │
  ▼
Node.js
  │
  ▼
PostgreSQL
```

Weaknesses:

- Single Point of Failure
- No Cache
- No CDN
- No Scaling

---

## Redesigned Architecture

```text
Users
 │
 ▼
CloudFront CDN
 │
 ▼
Application Load Balancer
 │
 ├── Node 1
 ├── Node 2
 ├── Node 3
 └── Node 4
        │
        ▼
Redis Cache
        │
        ▼
PgBouncer
        │
        ▼
PostgreSQL Primary
      /       \
 Replica1   Replica2

        │
        ▼
      SQS
        │
        ▼
Payment Worker
```

---

## Component Justification

| Component | Failure Prevented | How |
|------------|------------------|------|
| CloudFront | NIC Saturation | Serves images from edge |
| ALB | Single Point Failure | Distributes traffic |
| Redis | DB Overload | Caches reads |
| PgBouncer | Pool Exhaustion | Connection pooling |
| SQS | Payment Delays | Async processing |
| Replicas | Read Overload | Offload reads |
| Auto Scaling | CPU Saturation | Adds servers |