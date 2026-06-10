# swiggy-scale

# Swiggy Scale Simulation & Incident Architecture

A large-scale system design and Site Reliability Engineering (SRE) analysis of a Swiggy-like food delivery platform facing a massive traffic spike during an India vs Pakistan World Cup Final promotion.

---

## Scenario

SwiftEats is running on:

- Single Node.js Server
- Single PostgreSQL Database
- No Cache
- No CDN
- No Load Balancer
- Synchronous Payment Processing

A 50% discount notification is sent to 180 million users.

Approximately 10 million users attempt to use the application simultaneously, generating around 500,000 requests per second.

This project analyzes how the system fails, redesigns the architecture to survive the load, estimates AWS costs, and provides an operational incident runbook.

---

## Repository Documents

| Document | Description |
|-----------|-------------|
| FAILURE-CASCADE.md | Failure analysis, capacity calculations, RPS calculations, and incident timeline |
| ARCHITECTURE.md | Redesigned scalable architecture with component justifications |
| COST-ESTIMATE.md | AWS pricing estimates for baseline and peak traffic |
| RUNBOOK.md | Incident response procedures and operational runbook |

---

## Key Findings

- Peak traffic reaches approximately 500,000 Requests Per Second.
- PostgreSQL connection exhaustion occurs before Node.js saturation.
- Synchronous payment processing significantly amplifies database contention.
- Lack of CDN support causes static asset traffic to overwhelm application servers.
- Redis caching and asynchronous payment processing eliminate major bottlenecks.

---

## Architecture Overview

The redesigned architecture introduces:

- CloudFront CDN
- Application Load Balancer
- Multiple Node.js Instances
- Redis Cache
- PgBouncer Connection Pooling
- PostgreSQL Primary + Read Replicas
- Amazon SQS Payment Queue
- Payment Worker Service
- Auto Scaling

This architecture removes single points of failure and supports large-scale traffic spikes.

---

## Technologies Covered

- Node.js
- PostgreSQL
- Redis
- AWS CloudFront
- AWS Application Load Balancer
- AWS EC2
- AWS RDS
- AWS ElastiCache
- AWS SQS

---

## Outcome

The redesigned system costs approximately:

- Baseline: ~$1024/month
- World Cup Peak Event: ~$455 additional cost

Compared to an estimated outage loss of ₹189 crore, investing in scalable infrastructure provides a significant business advantage.