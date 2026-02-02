# Cloud Databases

## Why Cloud Databases?

Traditional on-premises databases require:
- Hardware purchase and maintenance
- Software installation and updates
- Backup management
- Disaster recovery planning
- Scaling for growth

**Cloud databases** handle all this for you.

---

## Benefits of Cloud Databases

| Benefit | Description |
|---------|-------------|
| **No Hardware** | Provider manages servers |
| **Scalability** | Grow/shrink on demand |
| **High Availability** | Automatic replication |
| **Backup & Recovery** | Automated backups |
| **Pay-per-Use** | Pay only for what you use |
| **Global Reach** | Deploy anywhere in the world |

---

## Major Cloud Database Providers

### Amazon Web Services (AWS)

**Amazon RDS (Relational Database Service)**
- Supports PostgreSQL, MySQL, SQL Server, Oracle
- Automatic backups and updates
- Multi-AZ deployment for high availability

**Amazon Aurora**
- MySQL and PostgreSQL compatible
- 5x faster than standard MySQL
- Auto-scaling storage

### Google Cloud Platform (GCP)

**Cloud SQL**
- Supports PostgreSQL, MySQL, SQL Server
- Easy migration from on-premises
- Automatic replication

**Cloud Spanner**
- Globally distributed
- Horizontally scalable
- Strong consistency

### Microsoft Azure

**Azure SQL Database**
- SQL Server compatible
- Built-in AI optimization
- Serverless option

**Azure Database for PostgreSQL**
- Managed PostgreSQL
- Flexible server options

---

## Database as a Service (DBaaS)

Cloud databases are often called **DBaaS**—the provider manages:

| Provider Manages | You Manage |
|------------------|------------|
| Hardware | Schema design |
| Operating system | Data |
| Database software | Queries |
| Backups | Security policies |
| Updates | Application code |
| Monitoring | Access control |

---

## Connecting to Cloud Databases

Connection is similar to local databases, but over the internet:

```python
# Example: Connecting to AWS RDS PostgreSQL
import psycopg2

conn = psycopg2.connect(
    host="mydb.123456789.us-east-1.rds.amazonaws.com",
    database="myapp",
    user="admin",
    password="secretpassword",
    port=5432
)
```

### Security Considerations

- Use **SSL/TLS** for encrypted connections
- Configure **firewall rules** (Security Groups, IP allowlists)
- Use **strong passwords** or IAM authentication
- Enable **audit logging**

---

## Cost Considerations

Cloud database pricing typically includes:

| Component | Description |
|-----------|-------------|
| **Instance hours** | Compute time |
| **Storage** | GB per month |
| **I/O** | Read/write operations |
| **Backup storage** | Beyond included amount |
| **Data transfer** | Outbound traffic |

### Cost Optimization Tips

1. **Right-size instances** - Don't over-provision
2. **Use reserved instances** - Commit for discounts
3. **Auto-pause** - Stop when not in use
4. **Monitor usage** - Track and adjust

---

## Migration Strategies

### Lift and Shift
Move existing database as-is to the cloud.
- Fastest approach
- Minimal changes required

### Re-platform
Make minor optimizations during migration.
- Use managed services
- Update connection strings

### Re-architect
Redesign for cloud-native features.
- Microservices architecture
- Serverless databases
- Most effort, most benefit

---

## Hybrid Approaches

Many organizations use a **hybrid** model:
- Critical data on-premises
- Development/testing in cloud
- Disaster recovery in cloud
- New applications cloud-native

---

## Summary

- Cloud databases eliminate infrastructure management
- Major providers: AWS, Google Cloud, Azure
- Benefits: scalability, availability, cost efficiency
- Consider security, compliance, and cost factors
- Migration can be gradual or all-at-once

---

**Next:** [Introduction to NoSQL](nosql-intro.md)
