# INCIDENT RUNBOOK

## STEP 1 - DETECT

| Metric | Threshold |
|----------|------------|
| ALB 5xx | >5% |
| DB Connections | >80% |
| CPU | >80% |
| Redis Memory | >75% |
| Queue Depth | >10000 |
| P99 Latency | >2s |

---

## STEP 2 - TRIAGE

1. Check DB Connections
2. Check CPU
3. Check Redis Misses
4. Check Queue Depth

---

## STEP 3 - RESPOND

### DB Pool Exhausted

Increase pool size.

### CPU Saturation

Scale instances.

### Queue Backup

Increase workers.

### Redis Misses

Warm cache.

---

## STEP 4 - ROLLBACK

```bash
aws ecs update-service \
--cluster swiggy-prod \
--service api \
--task-definition PREVIOUS_STABLE_VERSION
```

Never rollback database schema.

---

## STEP 5 - POSTMORTEM

- Timeline
- Root Cause
- Impact
- What Worked
- What Failed
- Action Items
- Owner
- Due Date