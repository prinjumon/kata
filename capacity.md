# Cost
Cost calculation in AWS for a microservice that handles 50 concurrent users and a total of 10000 users
1. **Compute (EC2)**
2. **Load Balancer (ALB)**
3. **Database (RDS )**
4. **Redshift**
5. **Monitoring (CloudWatch)**

---

#### 1. **EC2  Calculation**:
Assume we need 2 t3.medium EC2 instances  and auto-scale up to 4 during peak times.

- **Instance cost** (t3.medium, 2 vCPUs, 4GB RAM, on-demand pricing):
  - Price per hour: $0.0416 per instance (in the US East (N. Virginia) region)
  - Hours in a month (24 hours * 30 days): 720 hours
  - 2 instances (average load): 2 * 0.0416 * 720 = **$59.90/month**
  - Additional cost for 4 instances during peak hours (assuming peak for 8 hours/day for 15 days):
    - 2 extra instances for peak: 2 * 0.0416 * 8 * 15 = **$10.00/month**
  - **Total EC2 cost**: $59.90 + $10.00 = **$69.90/month**

### 2. **Load Balancer (ALB)**
You will need an **Application Load Balancer** (ALB) to distribute traffic to microservices.

- **ALB cost**:
  - Fixed hourly cost: $0.0225/hour
  - 720 hours/month = $0.0225 * 720 = **$16.20/month**
  - **Data processed cost**: Depends on traffic (estimated at 1TB for 1 million users)
    - $0.008/GB for first 10TB
    - Data transfer cost: 1,000 GB * $0.008 = **$8.00/month**

  - **Total ALB cost**: $16.20 + $8.00 = **$24.20/month**

---

### 3. **Database (RDS)**

#### **RDS Calculation**:
- Assume a small RDS instance (e.g., db.t3.micro) for relational data:
  - On-demand cost: $0.0188/hour
  - 720 hours: 0.0188 * 720 = **$13.54/month**
  - **Storage**: Assume 50GB of storage.
    - $0.115 per GB/month * 50GB = **$5.75/month**
  - **I/O costs**: Minimal for small-scale usage.

  - **Total RDS cost**: $13.54 + $5.75 = **$19.29/month**

### 3. **Redshift (Serverless)**

-Redshift Serverless is a new option where you only pay for the compute and storage you use, without needing to provision or manage nodes.

#### a) **Compute Cost (Redshift Processing Units - RPUs)**
- Redshift Serverless charges based on the **Redshift Processing Unit (RPU)** used per hour.
- **Price per RPU hour**: $0.375/RPU-hour.
- Let's assume for moderate usage, you need **20 RPU-hours per day**:
  - 20 RPU-hours/day * 30 days = 600 RPU-hours/month
  - Cost: 600 * $0.375 = **$225.00/month** for compute.

#### b) **Storage Cost**
- Storage cost is the same as for RA3 nodes, **$0.024 per GB per month**.
  - For **1TB** (1,000GB): 1,000 * $0.024 = **$24.00/month**
  - For **10TB** (10,000GB): 10,000 * $0.024 = **$240.00/month**

#### **Total Cost (Serverless)**

For 1TB storage:
- Compute cost: $225.00/month
- Storage cost (1TB): $24.00/month
- **Total: $249.00/month**

### 5. **Monitoring (CloudWatch)**
You’ll use **CloudWatch** for monitoring, logging, and alerting.

- **CloudWatch logs**: $0.50 per GB ingested and $0.03 per GB archived.
- Assume 5GB of logs = 5 * $0.50 = **$2.50/month**
- Basic monitoring is free, but advanced custom metrics and alarms could add minor extra costs.

---

### **Total Cost Estimation**:

| **Service**              | **Monthly Cost**       |
|--------------------------|------------------------|
| Compute (EC2)             | $69.90                 |
| Load Balancer (ALB)       | $24.20                 |
| Database (RDS)            | $19.29                 |
| CloudWatch Monitoring     | $2.50                  |
| Redshift                  | $249.00                 |

### **Total Monthly Cost**:
- **With EC2 + RDS**: $69.90 + $24.20 + $19.29 + $13.54 + $2.50 + $249.00  = **$378.43/month**
