# AWS COST ESTIMATE

## Baseline Architecture

### EC2

4 × t3.medium

$0.0416 × 720 × 4

= $119.81

### RDS Primary

db.r6g.large

= $131.04

### Read Replicas

2 × db.r6g.large

= $262.08

### Redis

3 × cache.r6g.large

= $358.56

### Load Balancer

= $56.20

### CloudFront

= $85

### SQS

= $12

---

Total Monthly Cost

≈ $1024/month

---

## Peak Event Cost

### Additional EC2

20 × t3.2xlarge

≈ $26.62

### Temporary DB Upgrade

≈ $4.11

### CloudFront Surge

≈ $425

---

Peak Event Cost

≈ $455

---

## Business Justification

45-minute outage:

₹4.2 crore × 45

= ₹189 crore loss

Monthly infrastructure cost:

≈ $1024

Infrastructure is significantly cheaper than downtime.