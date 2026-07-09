# Multi-Region Disaster Recovery Plan using Amazon S3 CRR & Amazon RDS Cross-Region Read Replica

## Overview

This project demonstrates the implementation of a **Multi-Region Disaster Recovery (DR) Strategy** on AWS using:

- Amazon S3 Cross-Region Replication (CRR)
- Amazon RDS Cross-Region Read Replica
- Amazon Route 53 Failover Routing
- Automated Health Checks
- Multi-AZ Deployment

The solution ensures business continuity, minimizes downtime, and enables rapid failover during regional outages.

---

# Architecture Diagram

```mermaid
flowchart LR

A[Users]

A --> B[Amazon Route 53]

B --> C[Primary Region<br/>us-east-1]

B --> D[DR Region<br/>us-west-2]

subgraph Primary Region
E[S3 Primary Bucket]
F[RDS Primary Database]
end

subgraph Disaster Recovery Region
G[S3 Backup Bucket]
H[RDS Read Replica]
end

E -->|Cross-Region Replication| G

F -->|Cross-Region Replication| H

B --> E
B --> F

Route53HealthCheck[Health Check]
Route53HealthCheck --> B
```

---

# High-Level Disaster Recovery Architecture

```text
                Users
                   │
                   ▼
             Amazon Route 53
                   │
     ┌─────────────┴─────────────┐
     │                           │
     ▼                           ▼
Primary Region            DR Region
(us-east-1)               (us-west-2)

S3 Bucket  ─────────►  S3 Backup Bucket
      CRR Replication

RDS Primary ────────► RDS Read Replica
     Cross-Region Replication
```

---

# End-to-End Workflow

```mermaid
flowchart TD

A[Application]

A --> B[Primary S3 Bucket]

A --> C[Primary RDS]

B --> D[S3 CRR]

D --> E[Backup S3 Bucket]

C --> F[RDS Cross-Region Replica]

F --> G[DR Database]

H[Route53 Health Check]

H --> I{Primary Healthy?}

I -->|Yes| A

I -->|No| G
```

---

# Disaster Recovery Components

| Service | Purpose |
|----------|----------|
| Amazon S3 | Object Storage |
| S3 CRR | Cross-Region Replication |
| Amazon RDS | Database Hosting |
| RDS Read Replica | Disaster Recovery Database |
| Route 53 | DNS Failover |
| CloudWatch | Monitoring |
| IAM | Replication Permissions |
